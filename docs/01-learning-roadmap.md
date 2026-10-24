# IntentFlow Learning Roadmap

The roadmap is learning-first: **85% learning / 15% building**. The 8–10 week estimate is a guide, not a deadline. Move forward when the learning question and evidence for a phase are clear. Build only the smallest artifact needed to test what you learned.

For each topic, record: learning question → concept → small experiment → implementation choice → evidence/result (pending until measured) → failure/surprise → next change.

| Phase | Learn first | Build only to test it | Exit evidence |
|---|---|---|---|
| 0. Define experiment | Implicit feedback, next-item prediction, session recommendation, leakage, offline evaluation limits | Specification, metrics, split/sessionization decision log | Written protocol defines prediction points, chronology, candidate policy, and evidence standard |
| 1. Understand data | RetailRocket schema, event semantics, sparsity, anonymous users, hashed properties, Parquet, DuckDB, data quality | Small inspection/validation pipeline; canonical event representation | Data profile, quality findings, assumptions, reproducible temporal boundaries |
| 2. Sessions and baselines | Session boundaries, timestamp ordering, popularity, co-visitation, Recall/NDCG/MRR/coverage | Session builder and baseline evaluators | Reproducible baselines on leakage-controlled split |
| 3. Candidate retrieval | Candidate recall, co-visitation, retrieval vs. ranking; embeddings/ANN only after baseline | Candidate generator interface and recall analysis | Retrieval contribution measured; ANN optional |
| 4. Ranking and features | Ranking losses, negative sampling, feature availability, LightGBM, ablations | Small ranker if candidate data supports it | Comparable fixed-path result and documented evidence |
| 5. Intent and routing | State estimation, confidence, intent shift, cost-aware policy, fallback | Interpretable intent signals and fixed-vs-adaptive router | Route reasons and ablation protocol inspectable |
| 6. Streaming and state | Event streams, ordering/idempotency, Redis state, offline/online consistency | Redpanda replay consumer and Redis updates | Duplicate/late/out-of-order behavior defined; local Kafka-compatible role documented |
| 7. Replay and serving | Replay limits, deterministic perturbations, FastAPI, Docker Compose, API contracts | Replay driver, API, basic demo | Clean replay and chosen perturbations reproducible; events update recommendations |
| 8. Performance and release | Load tests, latency percentiles, workload design, trade-offs, reproducibility | Measurements, Failure Lab report/view, final docs | p95 with workload disclosed; findings, failures, limitations, and run instructions published |

## Learning gates

- Do not start with streaming infrastructure or deep learning. First establish event meaning, sessionization, temporal evaluation, and baselines.
- Add a ranker only after candidate generation and its evaluation are understood.
- Add learned intent, embeddings, ANN, or neural models only with a named hypothesis and ablation.
- Keep Kubernetes, feature-store products, ClickHouse, and other datasets out of V1.
- If time is constrained, finish phases 0–4, then minimal intent/routing, replay, and measured serving. Reduce optional models before reducing data integrity or honest evaluation.

## Planned learning notes

These are future topics, not claims that work is complete.

```text
docs/notes/01-retailrocket-data.md
docs/notes/02-sessionization-and-temporal-splits.md
docs/notes/03-recommendation-metrics-and-baselines.md
docs/notes/04-candidate-retrieval.md
docs/notes/05-ranking-and-feature-design.md
docs/notes/06-intent-and-adaptive-routing.md
docs/notes/07-streaming-and-session-state.md
docs/notes/08-replay-and-robustness.md
docs/notes/09-serving-and-latency.md
docs/notes/10-failures-and-limitations.md
```
