# Version 2 — Real-Time Session Recommender

**Earlier framing:** A production-style session recommender with a portfolio-sized 8–10 week V1  
**Status:** Historical scope reduction; superseded by IntentFlow

## Project idea

Use the current anonymous behavioral session to recommend the next item when long-term user history is sparse. RetailRocket is the sole initial dataset; its short sessions, sparse purchases, implicit feedback, and hashed item properties become explicit constraints.

## Core question

Can recent session behavior produce useful next-item recommendations without relying on durable user profiles?

## Proposed 8-phase progression

1. Data and EDA.
2. Popularity and co-visitation baselines.
3. Candidate retrieval (co-visitation, embeddings/FAISS as appropriate).
4. Ranking (LightGBM first; PyTorch later only if justified).
5. Real-time events and Redis state.
6. FastAPI serving and Docker.
7. Temporal/replay evaluation and load measurement.
8. Small demo and portfolio polish.

## Intended deliverable

A Dockerized local project with an API, event pipeline, small UI, recommendation metrics (Recall@20, NDCG@10, coverage), latency measurements, architecture/trade-off documentation, failures, and learning notes. Offline replay replaces unsupported live-user A/B claims.

## Scope corrections

Cut Criteo, MIND, MovieLens, mandatory PyTorch ranking, and A/B testing from V1. Do not add technologies merely because large companies use them. A suggested p95 <100 ms target was a provisional objective, not a measured result.

## Why this version changed

The plan was still mostly framed as a conventional recommender build. To distinguish the portfolio project, its main question needed to involve changing session intent and the decision about when more computation is worth its latency cost.

No measured metrics were established in this version.
