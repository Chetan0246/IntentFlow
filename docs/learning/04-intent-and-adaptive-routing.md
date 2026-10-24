# Lesson 4 — Intent Trajectory and Adaptive Routing

## Learning goals

Turn vague “intent AI” language into measurable session signals. Compare adaptive routing with a fixed baseline and measure both quality and cost.

## Intent is a proxy

RetailRocket does not provide a ground-truth label for “exploring,” “comparing,” or “ready to buy.” These are proposed operational states. Do not call them verified user psychology. Instead, define measurable signals that might represent changing session behavior:

- session depth;
- recent item/category concentration when category data is available;
- how much the recent window differs from the earlier session;
- repeated views or progression to cart/transaction events;
- confidence based on history length/support.

Write down the formula and windows before running the comparison. Avoid building a classifier to reproduce labels you invented.

## A simple intent trajectory experiment

First make an interpretable state vector rather than a complicated model. Example questions:

- Is the visitor broadening or narrowing the set of categories recently viewed?
- Is the latest behavior consistent with earlier events?
- Has the event mix changed from views toward cart/transaction?
- How much evidence is available, and how uncertain is the state?

A state may be represented as features and confidence, not necessarily discrete “stages.” If you use stages, define deterministic rules, acknowledge uncertainty, and test sensitivity.

## Adaptive routing

Start with fixed paths:
- popularity for all sessions;
- co-visitation for all sessions with enough history;
- strongest fixed retrieval/ranking path.

Then define a simple policy that chooses between eligible paths based on session evidence. For example, short sessions take the cheap path; longer coherent histories may invoke richer retrieval. Add a latency fallback only after measuring the paths.

A fair comparison requires identical data splits, target definitions, and candidate rules. Record chosen route, reason, fallback, latency, and result. Report results by session strata and route as well as overall.

## Quality–latency trade-off

Measure end-to-end latency under a declared workload. Compare policies at several candidate/model complexity settings. An adaptive policy is useful only if it improves an explicit trade-off (such as quality at a fixed latency budget or lower latency with acceptable quality); more routing logic alone is not an improvement.

## Exercises

1. Construct two session prefixes with equal length but different recent-interest shifts.
2. Define a numeric “recent shift” signal and calculate it on a tiny example.
3. Write a deterministic rule for selecting popularity or co-visitation; list failure cases.
4. Explain why more events do not always mean higher intent confidence.
5. Design an ablation: remove intent signals but keep routing; then remove routing but keep one fixed path.
6. Make a table with quality, p95 latency, route share, and fallback share; leave values blank until measured.

## Build tickets

- **I1:** Create a small state calculator with documented formulas.
- **I2:** Log state values and route reasons for a replay session.
- **I3:** Compare fixed vs. adaptive policy on validation data.
- **I4:** Ablate intent features and routing separately.
- **I5:** Decide whether a latency fallback is justified by observed timings.

## Study material

- [Google ML recommendation overview](https://developers.google.com/machine-learning/recommendation/overview)
- [Google ML candidate generation](https://developers.google.com/machine-learning/recommendation/overview/candidate-generation)
- [Evaluation of Session-based Recommendation Algorithms](https://arxiv.org/abs/1803.09587), especially the comparison between simple and complex approaches.
- Treat papers using a different dataset or label definition as research context, not direct evidence for this project's hypothesis.
