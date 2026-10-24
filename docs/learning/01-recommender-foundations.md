# Lesson 1 — Recommender and Evaluation Foundations

## Learning goals

By the end, explain the next-item task, the difference between retrieval and ranking, implicit feedback, a temporal holdout, and why an offline score does not prove business impact.

## Core concepts

### What is the prediction?

At time (t), the system sees an eligible session prefix (S_t = [i_1, i_2, dots, i_t]) and produces an ordered list of items. The evaluation target is the next observed item (i_{t+1}). The target must not be used in session state, candidate generation, feature values, training labels for that prediction, or routing.

This is a prediction task over logged behavior. It is not automatically a measure of what a person truly wanted: the logs contain what was observed, and what was exposed/available may be unknown.

### Implicit feedback

A view, cart addition, or transaction is an observed interaction, not an explicit rating. The event types differ in meaning and frequency. A purchase is stronger evidence of an outcome than a view, but purchases are much rarer. Decide how to define the prediction target before choosing weights or combining events; document alternatives instead of silently assigning arbitrary weights.

### Retrieval and ranking

Retrieval narrows a large catalogue into a manageable candidate set. Ranking scores/order those candidates using richer session and item features. A candidate that retrieval omitted cannot be rescued by the ranker. Measure candidate recall separately from ranking quality.

### Offline metrics

For one next-item target (y) and recommendation list (R_K):

- Recall@K is 1 if (y) appears in the first K items, otherwise 0; average this indicator over eligible examples.
- Reciprocal rank is (1/rank(y)) when the target is retrieved, otherwise 0. MRR is its average.
- DCG gives more credit to relevant items near the top; NDCG normalizes DCG against the ideal ranking. For one target, the discount is driven by its position.
- Coverage is the number of distinct recommended items divided by the eligible catalogue size (state the exact catalogue definition).

Metrics answer different questions. A popular-item system may have reasonable recall but poor coverage. Never compare metrics from different candidate universes or split protocols as if they were directly comparable.

### Temporal leakage

A random row split can put future events in training while earlier events are in test. This lets the system learn from the future. Prefer a chronological split for this project, then ensure every prediction uses only earlier information. Fit popularity and co-visitation statistics using training history only.

### Retrieval quality vs. ranking quality

Candidate Recall@K asks whether the target appears among generated candidates. NDCG@10 asks where the ranker places the target after candidate generation. Report both when possible; otherwise a poor end result can hide whether retrieval or ranking is responsible.

## Hand exercise

Given the ordered events ((A, 10), (B, 12), (C, 18), (D, 25)), score the prediction after observing B.

1. Which events are visible?
2. What is the target?
3. Is a feature calculated from C allowed?
4. If recommendations are [X, C, Y], what are Recall@3 and reciprocal rank?
5. If a random split trained on D while testing the prediction after B, what leakage risk exists?

Write the answers before asking Codex to check them.

## IntentFlow decisions to make later

- Which event types count as prediction targets?
- Is the task next event, next distinct item, or next view?
- Which items are eligible candidates? Are already-seen items allowed?
- What counts as an eligible session/prediction point?
- How are ties and multiple events at one timestamp handled?

These choices change the metric; write them down before the final experiment.

## Build artifact

A protocol note containing the prediction-point definition, target, candidate policy, metrics, split design, and leakage checklist. No model code is required for this lesson.

## Study material

1. Read the [Google recommendation overview](https://developers.google.com/machine-learning/recommendation/overview) for retrieval/ranking stages.
2. Read [Evaluation of Session-based Recommendation Algorithms](https://arxiv.org/abs/1803.09587) for why simple methods are important comparators.
3. Write down what the paper's evaluation setup does and does not establish; do not assume its exact protocol is automatically right for our dataset.
