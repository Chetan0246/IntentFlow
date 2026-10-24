# IntentFlow — Project Specification

**Subtitle:** Adaptive Real-Time Session Recommendation Under Latency Constraints  
**Status:** Approved project direction; documentation stage  
**Planning estimate:** approximately 8–10 weeks, paced by learning and available time

## 1. Problem

Recommend the next relevant item from a visitor's current behavioral session, especially when persistent user identity and long-term history are sparse or unavailable. The system should update as events arrive and should not spend the same compute on every session regardless of how informative that session is.

RetailRocket is the sole V1 dataset. Its anonymous visitors, short sessions, sparse purchase events, implicit feedback, and hashed item properties are constraints to study and document.

## 2. Research question

> Can dynamically modeling a user's evolving session intent improve next-item recommendation, while allowing the system to adapt its computational cost to the available latency budget?

Supporting questions:

1. How much does recent within-session behavior help next-item prediction when long-term profiles are sparse?
2. Do intent-trajectory signals (exploring, narrowing, comparing, committing) add value beyond static session features and recommendation baselines?
3. Can a router select a low-cost path for short or low-confidence sessions and a richer path when evidence warrants it?
4. What quality, coverage, robustness, and latency trade-offs emerge under clean and controlled perturbed events?

These are open questions. Intent-state definitions and routing rules must be specified before comparisons and refined only with recorded evidence.

## 3. What makes this project distinct

IntentFlow does not claim to invent intent modeling or a new recommendation algorithm. Its identity is a focused experiment around:

- **Intent trajectory:** represent how session intent changes, not only which items were clicked.
- **Adaptive routing:** select an eligible retrieval/ranking path according to session evidence and measured latency budget, with a documented fallback.
- **Deterministic replay:** compare policies through historical-event replay without implying live users or A/B tests.
- **Robustness and quality–latency frontier:** evaluate recommendation quality together with system cost, including controlled perturbations.

Intent stages are operational hypotheses, not psychological ground truth supplied by RetailRocket.

## 4. Learning/building contract

Maintain **85% learning / 15% building**. Every component must answer a learning question or solve an observed problem. For every phase, record:

1. What you need to learn and why.
2. The concept and a small isolated experiment.
3. The implementation choice and alternatives.
4. Evidence, measured results, and limitations (leave pending until measured).
5. Failures/surprises and what you changed.

Do not start with streaming infrastructure or ML modeling before defining event semantics, sessionization, leakage controls, baselines, and evaluation.

## 5. Focused V1

### In scope

- RetailRocket only; document event semantics, data quality, and limitations.
- Reproducible raw-to-canonical event processing, temporal split, sessionization, and offline features.
- Popularity and item-to-item co-visitation baselines.
- Candidate generation and a two-stage retrieval/ranking path; LightGBM is the first learned ranker if the candidate/features support it.
- Interpretable intent state/trajectory and adaptive routing compared with fixed-strategy ablations.
- FastAPI event/recommendation endpoints; Redis current session state; PostgreSQL catalog/application metadata; Redpanda local Kafka-compatible event stream; Docker Compose.
- Deterministic replay, controlled robustness scenarios, small demo UI, and load/latency measurement.
- Design trade-offs, failure notes, limitations, and reproducible run instructions.

### Optional experiments, only after the core path

- Learned session/item embeddings and ANN retrieval (FAISS is an implementation option, not the project identity).
- Two-tower retrieval, neural ranker, learned intent estimator, or more advanced confidence calibration.

Include an optional experiment only when it has a named hypothesis and an ablation. Report its incremental quality, latency, complexity, and learning value.

### Out of scope for V1

- Criteo, MIND, MovieLens, cross-domain claims, or a general-purpose platform.
- Live-user A/B tests, production traffic, or claims of causal business impact.
- Kubernetes, ClickHouse, full feature-store deployment, microservice proliferation, or large dashboard suite.
- Mandatory transformers, neural ranking, or embedding retrieval.

## 6. Architecture

Three logical planes keep the system explainable at this scale.

### A. Offline learning

```text
RetailRocket → raw Parquet → validation/canonical events → sessionization
             → features → popularity/co-visitation/retrieval/ranking experiments
             → temporal offline evaluation → versioned artifacts
```

Use Parquet and DuckDB for local analytical work. Split chronologically; create features only from data available before each prediction point. Keep raw data out of version control unless license and size permit it.

### B. Online serving

```text
Demo/API → FastAPI → event path (Redpanda) → consumer → Redis session state
                   → recommendation path → session context → intent estimator
                   → adaptive router → popularity / co-visitation / optional ANN
                   → candidate pool → LightGBM ranker → filters/top-K → response
```

PostgreSQL holds catalog/application metadata; Redis holds current session state. Redpanda is the local Kafka-compatible implementation. Docker Compose runs a small environment (API, worker, Redpanda, Redis, PostgreSQL, and demo as needed). Keep logical code boundaries without splitting every component into a service.

### C. Replay laboratory

```text
time-ordered historical events → replay driver → event stream/online pipeline
                               → recommendation at each eligible point
                               → observed next event → evaluation/diagnostics
```

Use a clean control and deterministic perturbations such as duplicates, delays, out-of-order events, noisy clicks, and intent shifts where feasible. Label them as simulations. Hide the target event until after the recommendation is produced.

### Intent state and routing

Start with interpretable state (session depth, category concentration where available, stability, recent-interest shift, confidence). The router may use popularity for short/low-confidence sessions, co-visitation when history is useful, and optional ANN for richer sessions. Apply a measured latency budget and cheaper fallback. Record route, reason, fallback, candidate source, and timing.

## 7. Evaluation protocol

### Leakage and data controls

- Use chronological train/validation/test boundaries; record exact timestamps and exclusions after data inspection.
- Define session boundaries, event ordering, timestamp ties, and inactivity threshold before final comparison.
- At each prediction point expose prior events only. Fit popularity, co-visitation, encoders, features, and rankers on training data; tune on validation; report test once for the selected protocol.
- State candidate universe, seen-item policy, eligible sessions, rare-item handling, and treatment of sessions with no valid candidates.
- Report aggregate results and session strata (short/normal/rich when sample sizes support them), including denominators.

### Comparisons and metrics

Compare popularity, co-visitation, the strongest fixed retrieval/ranking path, and adaptive IntentFlow. Ablate intent signals and adaptive routing under the same protocol.

- Recommendation quality: **Recall@20, NDCG@10, MRR**
- Catalog: **coverage**
- Systems: **p50/p95/p99 end-to-end latency**, throughput, memory where feasible
- Adaptation: route distribution, fallback rate, quality per route/stratum, quality–latency or quality–compute frontier
- Robustness: clean control versus each defined perturbation

Do not invent results or targets. A prior sketch suggested p95 below 100 ms as a provisional objective; validate it on local hardware rather than treating it as promised. Record hardware, workload, concurrency, warm-up, sample count, and measurement method. Replay is offline evidence, not an A/B test.

## 8. Deliverables

- Reproducible preparation, training/evaluation, replay, serving, and local-run instructions.
- FastAPI API, event-driven session updates, and small interactive demo.
- Measured baseline/ablation tables for recommendation quality, coverage, and latency; mark unrun work pending.
- Architecture and trade-off notes.
- Failure Lab report/view: expected/observed next item, recommendation, intent state, route/source, relevant ranking signals.
- Learning notes in `docs/` showing what was learned and why each component exists.

## 9. Acceptance criteria

V1 is complete when:

1. Only RetailRocket is used; 85/15 contract and learning notes are visible.
2. Canonical events and sessions are reproducible; chronological boundaries, prediction-time feature availability, leakage checks, and metric definitions are documented.
3. Popularity and co-visitation return valid recommendations and use the same held-out protocol.
4. Fixed/adaptive policies and intent/routing ablations are comparable; route reasons and fallbacks are inspectable.
5. Replay hides targets until prediction; clean control and implemented perturbations are reproducible and honestly labeled.
6. A local Docker Compose workflow runs the API and required stream/state/metadata services; events update session context and recommendations can be requested through a small demo.
7. Results include measured Recall@20, NDCG@10, coverage, and p95 latency under a described workload. No fabricated numbers. If p95 <100 ms is missed, report the result and trade-off honestly.
8. README explains question, architecture, run path, results, choices, failures, limitations, and replay limits so another person can reproduce the workflow.

The adaptive hypothesis does not need to win. A negative or inconclusive result is acceptable when the protocol is sound and the conclusion is honest.

## 10. Limitations

This is one public e-commerce dataset studied offline/replay. It cannot establish live conversion lift or causal business impact. Intent labels are inferred operational states, not observed mental states. Findings depend on event semantics, split/sessionization choices, candidate construction, local hardware, and replay assumptions. State these limits beside portfolio claims.
