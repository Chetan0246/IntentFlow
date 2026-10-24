# Lesson 5 — Backend, Streaming, and Replay

## Learning goals

Understand the online path and its failure modes before operating the services. Build the smallest API and state flow first; use a local Kafka-compatible stream only when its learning goal is clear.

## Online flow

A minimal serving request needs a session identifier and the recent events known for that session. The API calls a recommender interface and returns an ordered item list with optional explanation fields (candidate source, route, score metadata). Separate API transport from recommendation logic so the latter can be run offline too.

Redis can hold short-lived current session state. Define key shape, expiration, append/update behavior, and duplicate handling. PostgreSQL can hold catalog/application metadata, but don't duplicate every offline analytical table into it.

## Event streaming

A stream separates event production from consumption; it introduces ordering, retries, duplicate delivery, lag, and schema evolution concerns. For this V1, Redpanda is a local Kafka-compatible implementation. Learn the concepts (topic, partition, key, consumer, offset) rather than memorizing vendor commands.

Use visitor/session keying only after considering ordering and partitioning. Consumers should be safe if the same event is processed twice; define idempotency. A delayed/out-of-order event needs a clear policy. Don't assume “exactly once” removes application-level duplication problems.

## Replay semantics

Replay must be deterministic: the same source sequence and perturbation seed should produce the same order. At each prediction point:

1. deliver only the prefix available up to that point;
2. ask the recommender for candidates;
3. record recommendation, route, and timing;
4. only then reveal the next event as evaluation target;
5. update session state and continue.

This order prevents target leakage. A noisy-click or delay experiment should state exactly how events were transformed and preserve the clean replay for comparison. It is a simulation of pipeline behavior, not a causal experiment with people.

## Serving measurement

Measure the whole request path, not only model inference. Record warm-up, sample count, concurrency, hardware, and data size. Report percentiles rather than just averages. Use a local load tool only when the endpoint and workload have been defined.

## Exercises

1. Draw producer → topic → consumer → Redis/API event flow and label where an event can repeat.
2. Explain how a duplicate event could inflate session depth and change recommendations.
3. Define a deterministic event ID and idempotent update behavior.
4. Hand-replay three events and show exactly what the model sees before each target.
5. Compare model-only time and end-to-end request time on a tiny fake recommender.
6. List what a replay experiment can and cannot establish.

## Build tickets

- **S1:** Implement a pure in-memory event/session/recommendation path.
- **S2:** Add FastAPI routes around the pure functions.
- **S3:** Add Redis only after deciding its key and expiry semantics.
- **S4:** Add Redpanda and a consumer; document duplicate/order behavior.
- **S5:** Build deterministic replay using the same core functions.
- **S6:** Run a documented load workload; report p50/p95/p99.
- **S7:** Add Docker Compose last, when each service's role is understood.

## Study material

- [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/)
- [Redis Python client guide](https://redis.io/docs/latest/develop/clients/redis-py/)
- [Apache Kafka documentation](https://kafka.apache.org/documentation/)
- [Redpanda quickstart](https://docs.redpanda.com/current/get-started/quick-start/)
- [Docker Compose quickstart](https://docs.docker.com/compose/gettingstarted/)
- [Locust documentation](https://docs.locust.io/en/stable/)
