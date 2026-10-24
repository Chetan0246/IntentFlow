# IntentFlow Build Plan

This file turns the specification into learn-first deliverables. Follow it with the [self-guided manual](04-self-guided-manual.md), [experiment workbook](05-experiment-workbook.md), and [implementation blueprint](06-implementation-blueprint.md). Phases can take longer than the 8–10 week estimate; do not skip a learning gate to meet a date.

## You own the project

You read, reason, make protocol/design choices, implement, run experiments, save actual outputs, and write the final claims. The docs give you study material, exercises, expected behavior, answer keys, exit gates, and troubleshooting steps. Optional outside help can review your work, but the plan does not depend on it.

## Phase plan

| Phase | Learn | You build | Exit gate |
|---|---|---|---|
| 0. Experiment design | Lesson 1; next-item prediction, temporal leakage, metrics | Written protocol: target, prediction point, split, eligibility, candidate policy | Protocol explicit before dataset analysis |
| 1. Data understanding | Lesson 2; file formats, event semantics, profiling | Data dictionary, profile tables, validation checks; keep raw files unchanged and untracked | Actual schema and data quality documented |
| 2. Sessions | Lesson 2; ordering, inactivity gaps, timestamp ties | Sessionization rule, implementation, threshold sensitivity note | Hand-worked examples match code and are reproducible |
| 3. Baselines | Lessons 1 and 3; popularity, co-visitation, metrics | Popularity/co-visitation generators and offline evaluator | Both baselines evaluated on identical chronological protocol |
| 4. Retrieval/ranking | Lesson 3; candidate recall, negatives, feature timestamps, LightGBM | Candidate table, recall analysis, ranker if justified, ablations | Candidate and ranking behavior understood separately |
| 5. Intent/routing | Lesson 4; operational state signals, confidence, policy cost | State calculator, route rules, logs, fixed/adaptive comparison | Route decisions auditable; no unsupported intent claim |
| 6. API and state | Lesson 5; API design, Redis semantics | In-memory core first, FastAPI, then Redis if justified | Session event changes can be requested and inspected |
| 7. Streaming | Lesson 5; topic/partition/consumer/offset, order/idempotency | Redpanda producer/consumer and event update flow | Duplicate, delay, and ordering rules documented |
| 8. Replay and robustness | Lessons 4–5; offline replay limits | Deterministic replay, clean control, chosen perturbations | Same input/seed reproduces trace; targets hidden at scoring |
| 9. Load, UI, release | Lesson 6; latency percentiles, reproduction | Small demo, load workload, final docs/results/failures | Project acceptance criteria are met or gaps are stated |

## Daily task card

At the start:

```text
Today I want to understand:
My current explanation:
One question I will answer:
Small exercise/build task:
What evidence will count:
```

At the end:

```text
What I can now explain:
What I built or inspected:
Observed output (or pending):
What surprised me:
Next session:
```

## First 10 study days

These are learning days; most require no project code.

1. Define next-item prediction using a five-event toy session; mark visible prefix and target.
2. Explain implicit feedback and what a view/cart/transaction does and does not establish.
3. Draw retrieval → ranking → re-ranking and name a failure for each.
4. Calculate Recall@K and reciprocal rank by hand.
5. Explain temporal leakage with an event timeline; choose a provisional chronological split.
6. Read the RetailRocket dataset card; note files, fields, event types, temporal-property caveat, and license.
7. Design a data-profile table; then inspect headers/counts after obtaining data.
8. Define sessionization with a toy visitor and two candidate inactivity gaps.
9. Hand-compute popularity and co-visitation for tiny made-up sessions.
10. Fill the [experiment protocol worksheet](05-experiment-workbook.md#protocol-worksheet) and self-audit it before modeling.

Continue with the build tickets in each lesson after the matching gate. A phase is done only when you can explain its central concept without copying, reproduce its small experiment, and state at least one limitation. Code that runs without understanding is not an exit gate.
