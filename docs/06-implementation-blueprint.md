# IntentFlow Implementation Blueprint

This is a map for building the software yourself. It defines suggested boundaries and data/API contracts; it is not generated application code. Change the layout only when you can explain why.

## Suggested repository layout

```text
intentflow/
├── apps/
│   ├── api/                 # HTTP transport and request/response models
│   ├── replay/              # Deterministic historical-event driver
│   └── demo/                # Small user-facing interaction flow
├── data/
│   ├── raw/                 # Local only; do not commit dataset files
│   ├── bronze/              # Parsed immutable source tables
│   ├── silver/              # Validated canonical events / sessions
│   └── gold/                # Features, candidates, evaluation tables
├── pipelines/
│   ├── ingestion/
│   ├── validation/
│   ├── sessionization/
│   └── features/
├── recommenders/
│   ├── popularity/
│   ├── covisit/
│   ├── retrieval/
│   ├── ranking/
│   └── routing/
├── intent/                  # State signals and confidence
├── evaluation/
│   ├── offline/
│   ├── replay/
│   ├── robustness/
│   └── load/
├── infra/
│   └── docker/
├── notebooks/               # Exploration only; production logic belongs in modules
├── docs/
└── README.md
```

Do not create every folder at the start. Add a folder when a phase produces code that belongs there. Keep data artifacts, configuration, and code separate.

## Canonical event contract

Start from a validated event table with these logical fields:

| Field | Meaning | Rule to decide |
|---|---|---|
| `event_id` | Stable identity for deduplication/replay | Source has no universal event ID; derive reproducibly from source row or a documented key |
| `visitor_id` | Anonymous visitor identifier | Preserve source value and type consistently |
| `event_time` | Event timestamp | Parse milliseconds explicitly, store timezone consistently |
| `event_type` | view, addtocart, transaction (confirm source values) | Reject or quarantine unknown values; do not silently map |
| `item_id` | Interacted item | Preserve as stable string or integer type |
| `transaction_id` | Optional purchase transaction | Null for non-transaction events |
| `source_file`, `source_row` | Lineage | Keep enough lineage to debug the source record |

Keep source rows immutable and retain a rejected-row report. Never use a target event to create features for that prediction.

## Pipeline contracts

Each stage should have a defined input, output, and invariants:

1. **Parse:** source rows → typed raw tables. Invariant: row counts reconcile to source plus explicit rejects.
2. **Validate:** raw → canonical accepted/rejected records. Invariant: required fields valid; reject reasons counted.
3. **Sessionize:** ordered visitor events → session IDs. Invariant: events inside a session satisfy the documented gap rule.
4. **Split:** sessions/prediction points → train/validation/test manifest. Invariant: each partition respects temporal policy.
5. **Feature build:** point-in-time session/item features. Invariant: `feature_time <= prediction_time`.
6. **Candidate generation:** prefix → unique candidate list with source and score. Invariant: target visibility is forbidden; no invalid IDs.
7. **Ranking:** candidate feature rows → ordered scores. Invariant: every score can be traced to candidate features.
8. **Evaluation:** predictions + held-out targets → metrics and denominators. Invariant: missing candidate is a miss unless the written protocol says otherwise.

## Recommender interface

A pure core function should accept a session prefix and a context/artifact bundle and return:

```text
RecommendationResult
  items: ordered list of item IDs
  candidate_sources: source(s) contributing each item
  route: selected policy
  route_reason: concise machine-readable reason
  fallback_used: boolean
  timing_ms: stage and total timings (when serving)
```

Keep it independent of FastAPI, Redis, and Redpanda so the same logic can run in offline evaluation, replay, and serving.

## HTTP contract (suggested)

### `POST /events`

Request:

```json
{
  "event_id": "replay-000123",
  "session_id": "session-abc",
  "visitor_id": "visitor-7",
  "event_time": "2026-01-15T10:12:00Z",
  "event_type": "view",
  "item_id": "item-42"
}
```

Response: accepted/rejected status, duplicate indication, and updated session version or event count. Define whether acceptance means stored locally or fully consumed by the stream.

### `GET /recommend?session_id=session-abc&k=20`

Response:

```json
{
  "session_id": "session-abc",
  "items": ["item-91", "item-17"],
  "route": "covisit",
  "route_reason": "history_support",
  "fallback_used": false,
  "generated_at": "2026-01-15T10:12:01Z"
}
```

The values above are illustrative, not results. Define empty-session behavior, unknown session behavior, invalid K, and whether previously seen items are filtered. Keep event IDs and session IDs opaque; don't expose personal data.

## Service responsibility

- **FastAPI:** validate requests, call core logic, return stable response shapes.
- **Redis:** short-lived online session state; define key format, TTL, event deduplication, and atomic update behavior.
- **PostgreSQL:** catalog/application metadata and small persistent records; not the offline analytics warehouse.
- **Redpanda:** local Kafka-compatible event transport; use only after in-memory flow is understandable.
- **Replay:** feed ordered historical events and collect outputs using the same core recommender.
- **Demo:** explain recommendations changing as items are interacted with; label replay data and limitations.

Docker Compose should be introduced after individual service responsibilities are clear. Avoid deploying Kubernetes in V1.

## Configuration and artifacts

Put split dates, session gap, candidate count, model settings, route thresholds, and perturbation seed in versioned configuration. Save each run's config with its metrics. Keep generated model/data files out of Git unless small and intentionally included. Record software versions and dataset version/checksum.

## Implementation order

1. Pure functions on tiny in-memory examples.
2. Data parser/profile and canonical event table.
3. Sessionization and temporal evaluator.
4. Popularity then co-visitation.
5. Candidate table, ranking if justified.
6. Intent signals and fixed/adaptive routing.
7. API around pure core.
8. Redis session state; then Redpanda consumer.
9. Deterministic replay through shared core.
10. Demo, measured load workload, documentation and final clean reproduction.
