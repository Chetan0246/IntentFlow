# IntentFlow Build Plan and Task Ownership

This file turns the specification into a sequence of learn-first deliverables. The phases can take longer than the 8–10 week estimate. Do not skip a learning gate to meet a calendar date.

## Ownership

| You (learner/primary builder) | Codex (tutor/reviewer) |
|---|---|
| Read the lesson material; write notes in your own words | Explain concepts, point to relevant sections, create small practice questions |
| Make and record protocol/design decisions | Compare options and flag risks such as leakage or unfair comparisons |
| Implement the code and run commands/experiments | Review your code, explain errors, offer hints or pseudocode |
| Keep actual metrics/logs and decide what they mean | Help interpret actual outputs and check claims against them |
| Decide whether optional features are worth the added complexity | Estimate trade-offs and help design ablations |

Codex should not implement a complete phase without a direct request. If you're stuck, ask first for a hint, then a small example, then a review of your attempt.

## Phase plan

| Phase | Learn | You build | Codex helps | Exit gate |
|---|---|---|---|---|
| 0. Experiment design | Lesson 1; next-item prediction, temporal leakage, metrics | Written protocol: target, prediction point, split, eligibility, candidate policy | Hand-check your protocol on toy events and identify leakage risks | Protocol is explicit before dataset analysis |
| 1. Data understanding | Lesson 2; file formats, event semantics, profiling | Data dictionary, profile tables, validation checks; keep raw files unchanged and untracked | Explain unfamiliar columns and review summaries you provide | Actual schema and data quality are documented |
| 2. Sessions | Lesson 2; ordering, inactivity gaps, timestamp ties | Sessionization rule, implementation, threshold sensitivity note | Review edge cases and compare alternatives | Hand-worked examples match code and are reproducible |
| 3. Baselines | Lessons 1 and 3; popularity, co-visitation, metrics | Popularity/co-visitation generators and offline evaluator | Help validate metric formulas and inspect real outputs | Both baselines evaluated on identical chronological protocol |
| 4. Retrieval/ranking | Lesson 3; candidate recall, negatives, feature timestamps, LightGBM | Candidate table, recall analysis, ranker if justified, ablations | Review feature leakage and experiment fairness | Candidate and ranking behavior are separately understood |
| 5. Intent/routing | Lesson 4; operational state signals, confidence, policy cost | State calculator, route rules, logs, fixed/adaptive comparison | Challenge assumptions; suggest failure cases and ablations | Route decisions auditable; no unsupported intent claim |
| 6. API and state | Lesson 5; API design, Redis semantics | In-memory core first, FastAPI, then Redis if justified | Review contracts, state expiry, duplicate behavior | Session event changes can be requested and inspected |
| 7. Streaming | Lesson 5; topic/partition/consumer/offset, order/idempotency | Redpanda producer/consumer and event update flow | Explain event-flow failure modes; review design | Duplicate, delay, and ordering rules documented |
| 8. Replay and robustness | Lessons 4–5; offline replay limits | Deterministic replay, clean control, chosen perturbations | Verify target reveal ordering and compare traces | Same input/seed reproduces trace; targets hidden at scoring |
| 9. Load, UI, release | Lesson 6; latency percentiles, reproduction | Small demo, load workload, final docs/results/failures | Review UI flow, workload description, and portfolio claims | Acceptance criteria in project spec are met or gaps are stated |

## Daily task card

At the start of a session, write:

```text
Today I want to understand:
My current explanation:
One question I will answer:
Small experiment/build task:
What evidence will count:
```

At the end, write:

```text
What I can now explain:
What I built or inspected:
Observed output (or pending):
What surprised me:
Next session:
```

## First 10 study days

These are learning days; most require no project code.

1. Define next-item prediction using a five-event toy session; mark the visible prefix and target.
2. Learn implicit feedback and list what a view/cart/transaction does and does not establish.
3. Draw retrieval → ranking → re-ranking and say what failure each stage can cause.
4. Calculate Recall@K and reciprocal rank by hand on toy examples.
5. Explain temporal leakage with an event timeline; choose a provisional chronological split rule.
6. Read the RetailRocket dataset card; write down files, fields, event types, temporal property caveat, and license.
7. Design a data profile table; then inspect headers/counts after obtaining data.
8. Define sessionization with a toy visitor and two candidate inactivity gaps.
9. Hand-compute popularity and co-visitation for tiny made-up sessions.
10. Write the experiment protocol and ask Codex to critique it before modeling.

After those days, continue to the build tickets in each learning lesson. Keep a short daily streak rather than trying to complete a whole phase in one sitting.

## Completion rule

A phase is done only when you can explain its central concept without copying, reproduce its small experiment, and state at least one limitation. Code that runs without understanding is not an exit gate.
