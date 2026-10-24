# Lesson 3 — Baselines, Retrieval, and Ranking

## Learning goals

Build a meaningful baseline before introducing complex models. Understand co-visitation, candidate recall, ranking features, and why a ranker cannot fix missing candidates.

## Baselines

A baseline is a simple reference that helps determine whether added complexity earned its cost.

### Popularity

Count eligible training interactions per item and recommend the top items. Define whether counts use all events or a chosen event type and whether they use a time window. The same training-only cutoff must apply to every test prediction. Popularity is a useful fallback for an empty/short session, but can reduce catalogue coverage.

### Item-to-item co-visitation

Count item pairs observed in the same session or within a chosen window. Given recent session items, accumulate associated items and rank them by a declared score. Decisions include pair directionality, distance/window, recency weighting, event weighting, minimum support, and how to combine signals from multiple session items. Begin with the simplest count-based version; change one choice at a time.

## Candidate generation

A generator returns a limited set of items likely to contain the next target. Combine sources only after each source is evaluated separately. Deduplicate candidates and preserve source/score metadata so later analysis can explain where a recommendation came from.

Evaluate candidate recall before ranking. Increase candidate count and see the trade-off in recall, latency, and memory. A learned embedding/ANN system is optional and should be attempted only if simpler candidates leave a measurable gap.

## Ranking

Each training example can be a session-prefix/candidate pair with a label indicating whether that candidate is the target. Features must be computed at the prediction point. Examples include co-visitation score, last-seen recency, number of supporting session items, item popularity from training, session length, and candidate source.

Negative candidates are not observed positives. Sampling them changes the training problem; document the sampling method and keep the same candidate construction for evaluation.

LightGBM can train ranking objectives such as LambdaRank/NDCG, but choosing an objective does not remove the need for valid query groups, labels, candidate sets, and leakage controls. For the first model, a straightforward ranker or binary scorer may be easier to understand. Compare the simplest viable model before adding ranking-specific complexity.

## Exercises

1. For sessions [A,B,C] and [A,B,D], calculate pair co-occurrence counts by hand.
2. Given session prefix [A,C], use a tiny pair-count table to produce top candidates and explain ties.
3. Make a candidate-recall example where the target is absent; explain why even a perfect ranker fails.
4. Define 3 positive and 5 negative candidate examples for one prediction point; state where negatives came from.
5. List every feature and its latest allowed timestamp.
6. Compare popularity and co-visitation on the same toy held-out targets.

## Build tickets

- **R1:** Implement popularity on training data only.
- **R2:** Implement directional or symmetric co-visitation (choose and document one).
- **R3:** Measure Recall@20 for candidate generation, using your agreed eligibility rules.
- **R4:** Build a candidate/label table with a documented negative-sampling strategy.
- **R5:** Train a first LightGBM model only after the data table and evaluation are understandable.
- **R6:** Run an ablation that removes one feature group and compare on the same validation/test protocol.

## Study material

- [Google ML candidate generation](https://developers.google.com/machine-learning/recommendation/overview/candidate-generation)
- [Google ML ranking](https://developers.google.com/machine-learning/recommendation/overview/ranking)
- [LightGBM documentation](https://lightgbm.readthedocs.io/en/stable/); read the objective and ranking/query-group sections before training.
- [Session recommendation evaluation paper](https://arxiv.org/abs/1803.09587): it motivates including simple neighbor methods in comparisons.
