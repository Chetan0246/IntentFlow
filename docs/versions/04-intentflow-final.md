# Version 4 — Final IntentFlow Plan

**Name:** IntentFlow  
**Subtitle:** Adaptive Real-Time Session Recommendation Under Latency Constraints  
**Status:** Current project direction

## Core research question

> Can dynamically modeling a user's evolving session intent improve next-item recommendation, while allowing the system to adapt its computational cost to the available latency budget?

This version combines a research question, learning plan, reproducible evaluation, and a finishable portfolio deliverable. It explicitly avoids recreating a hyperscale recommendation stack.

## Three logical planes

### Offline learning

RetailRocket → raw Parquet → validation/canonical events → sessionization → features → popularity/co-visitation/retrieval/ranking → temporal evaluation → versioned artifacts.

### Online serving

Demo/API → FastAPI → Redpanda event path → consumer → Redis session state → session context → intent estimator → adaptive router → popularity/co-visitation/(optional ANN) → candidate pool → LightGBM ranker → filters/top-K.

PostgreSQL stores catalog/application metadata. Docker Compose runs the local components. Redpanda is the local Kafka-compatible implementation; it is not a claim that Kafka is required.

### Replay laboratory

Replay time-ordered historical events through the online path, score before exposing the next event, and compare output with the observed next event. Controlled duplicate, delay, ordering, noise, and intent-shift cases are simulations. Replay is not a live A/B test.

## Adaptive routing hypothesis

A short/low-confidence session may use a cheap popularity fallback. A session with useful history can use co-visitation and ranking. A richer session may qualify for learned retrieval if that optional model is implemented. If a measured latency budget is at risk, use a cheaper fallback.

Record route decisions, reasons, candidate sources, fallback, and timing. Intent state is an interpretable operational representation, not a psychological label.

## Focused V1

- RetailRocket only; 85% learning / 15% building.
- Data validation, temporal split, sessionization, and leakage-safe features.
- Popularity and co-visitation baselines, candidate retrieval, and a conditional LightGBM ranker.
- Interpretable intent signals and fixed-versus-adaptive policy comparison.
- FastAPI, Redis, PostgreSQL, Redpanda, Docker Compose, replay, and a small demo.
- Offline quality/coverage metrics and measured latency; honest failure/limitation notes.

ANN/two-tower retrieval, neural rankers, learned intent, and advanced calibration are optional after the core path. Exclude extra datasets, live A/B testing, Kubernetes, ClickHouse, a full feature store, and microservice sprawl.

## Evaluation and completion

Use chronological train/validation/test boundaries; ensure prediction-time features cannot see the target. Compare popularity, co-visitation, the strongest fixed path, and adaptive policy, with intent/routing ablations. Report Recall@20, NDCG@10, MRR, coverage, p50/p95/p99 latency, throughput where feasible, route/fallback rates, and robustness under declared perturbations. Publish workload and local hardware for latency measurements.

V1 is complete when the data and replay protocol are reproducible, baselines and adaptive comparisons are measured on the same protocol, event-driven local serving/demo works, and README explains results, trade-offs, failures, limitations, and how to run it. The adaptive hypothesis need not win; the evidence and conclusion must be honest.

The 85/15 learning contract, detailed acceptance criteria, and protocol are maintained in the current [project specification](../00-project-specification.md). No fabricated result belongs in this history.
