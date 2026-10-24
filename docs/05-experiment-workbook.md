# IntentFlow Experiment Workbook

Work through the questions before implementation. Put your answers in your own learning notes. The answer key is at the end; attempt each exercise first.

## A. Prediction-point exercise

Ordered events for a session:

| Position | Time | Item |
|---:|---:|---|
| 1 | 10:00 | A |
| 2 | 10:02 | B |
| 3 | 10:03 | C |
| 4 | 10:05 | D |

For a prediction immediately after position 2:

1. Which item history is visible?
2. What is the target?
3. Can a category or popularity feature computed from position 3 be used?
4. If the output is [X, C, Y], what are Recall@3 and reciprocal rank?
5. If the target does not appear in the candidate set, can the ranker recover it?

## B. Metric exercise

There are four eligible prediction points. At K=3, the targets are hit in ranks 1, absent, 3, absent.

- Calculate Recall@3.
- Calculate MRR.
- Explain why Recall@3 and MRR tell different stories.
- How should a prediction with zero eligible candidates be handled? Write a rule and use it consistently.

## C. Sessionization exercise

Visitor 7 has events at 09:00 A, 09:08 B, 09:40 C, 10:05 D. Use a 30-minute inactivity gap:

1. Mark session IDs.
2. Repeat with a 20-minute gap.
3. Explain why neither threshold is “the true session” given only the logged events.
4. Name a sensitivity result that should be recorded.

## D. Co-visitation exercise

Training sessions are [A,B,C] and [A,B,D]. For simplicity, count each unordered pair once per session:

- List all pairs and counts.
- Given current session [A], rank co-visited items.
- Given [B], rank co-visited items.
- What tie-breaking rule would make output deterministic?
- If the target is E and E never co-occurs in training, what fallback can serve it?

## E. Temporal split and feature exercise

Suppose training ends at timestamp T. A test prediction is made at T+10 using session history before T+10. Item property P changes at T+5.

1. Is the property value after T+5 available at prediction time?
2. Can test-period interactions be used to build a popularity table for this prediction?
3. What should be saved to make the split auditable?

## F. Intent proxy exercise

Earlier events are [A,A,A] and recent events are [X,Y,Z]. Create one simple recent-shift signal on a toy example. State:

- formula;
- window length;
- required item/category information;
- behavior when category information is absent;
- why this is only a proxy, not a ground-truth psychological label.

## G. Routing exercise

Define two policies:
- fixed: always co-visitation if possible, else popularity;
- adaptive: choose co-visitation only when history support exceeds a threshold, otherwise popularity.

Write down:
- route conditions and tie behavior;
- metrics for comparing them;
- how to ablate intent while keeping retrieval constant;
- what output field explains each route decision.

## H. Replay exercise

For a five-event sequence, write the exact loop order for each prediction point. Add one duplicate event using a fixed perturbation seed. Explain how to preserve a clean control and how to ensure the target is revealed only after the recommendation is recorded.

## I. Protocol worksheet

Copy this and fill it before final comparisons:

```text
Task and target definition:
Event types included:
Prediction points / eligible examples:
Session gap rule:
Timestamp tie rule:
Train / validation / test boundaries:
Candidate catalogue:
Seen-item policy:
Candidate sources and candidate count:
No-candidate behavior:
Metric formulas / aggregation:
Feature list and latest allowed timestamp:
Baselines and adaptive comparisons:
Ablations:
Robustness perturbations and deterministic seed:
Latency machine and workload:
What result would support the hypothesis:
What result would weaken or falsify the hypothesis:
Known limitations:
```

## J. Answer key and reasoning

### A. Prediction point

1. Only A and B are visible.
2. C is the next-item target.
3. No. Position 3 occurs after the prediction point and contains the target itself.
4. Recall@3 = 1; reciprocal rank = 1/2 because C is second.
5. No. Ranking orders only supplied candidates; retrieval must include the target for it to be rankable.

### B. Metrics

- Recall@3 = 2 hits / 4 points = 0.5.
- MRR = (1 + 0 + 1/3 + 0) / 4 = 1/3.
- Both count the same two hits as relevant, but MRR rewards rank 1 more than rank 3.
- Zero-candidate handling must be decided before evaluation. A common choice is to retain the eligible prediction point as a miss and record the no-candidate rate; excluding it can inflate quality.

### C. Sessions

- At 30 minutes: 09:00 and 09:08 are session 1; 09:40 and 10:05 session 2 (the gap from 09:40 to 10:05 is 25 minutes).
- At 20 minutes: 09:00 and 09:08 session 1; 09:40 session 2; 10:05 session 3.
- Logs reveal inactivity, not a user's subjective session boundary.
- Record session count, length distribution, eligible next-item examples, and metric sensitivity under the alternative threshold.

### D. Co-visitation

- Session 1 pairs: AB, AC, BC. Session 2: AB, AD, BD. Counts: AB=2; AC=1; BC=1; AD=1; BD=1.
- From A, B ranks highest; C and D tie.
- From B, A ranks highest; C and D tie.
- Use stable item ID ordering as a deterministic tie-break (or specify another).
- Popularity is a possible fallback for an unseen target/item, though it cannot guarantee retrieval of E.

### E. Temporal split

1. No. At T+10 the value from T+5 is available only if the application has that timestamped update and joins it point-in-time.
2. No. The popularity table for a frozen train/test protocol is fitted on training history only.
3. Save exact boundaries, event filters, item-property as-of rule, timezone, tie policy, and dataset version/checksum.

### F. Intent proxy

Many formulas can be valid as experiments. Example: compute the fraction of items in the recent window whose category was absent from the earlier window. Define window sizes and handling of missing categories. If category data is unavailable, mark the signal missing or use an item-based proxy. The formula measures observed sequence change; it does not reveal mental state.

### G. Routing

A fair comparison fixes split, target, candidate eligibility, and load workload. Report quality, coverage, latency, route share, fallback share, and no-candidate rate. An intent ablation removes intent features while holding candidate sources and evaluation constant. Log a route reason such as `short_history_fallback` or `support_above_threshold`.

### H. Replay

At each step: expose prefix → request candidates/recommendations → record route and output → reveal next event as target → score → append/update state. A deterministic duplicate uses a fixed event ID/seed and is kept in a separate perturbed trace; retain the untouched control trace.

Answers to G and H are design choices. Write and justify yours; don't copy a single answer as universally correct.
