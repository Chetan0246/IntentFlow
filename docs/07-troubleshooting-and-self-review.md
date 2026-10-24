# Troubleshooting and Self-Review Guide

Use this before changing algorithms. First reduce the problem to the smallest reproducible example and write down what you expected versus what happened.

## Data and pipeline

| Symptom | Likely cause | First check |
|---|---|---|
| Timestamp appears near 1970 or far future | Milliseconds treated as seconds, or timezone conversion mistake | Verify raw unit and convert one known sample by hand |
| Session count explodes | Gap threshold too small, unsorted events, or timestamp tie order unstable | Inspect one visitor's ordered timeline and gap calculation |
| A session spans a huge period | Missing gap split or visitor/session key error | Print boundaries for a small visitor sample |
| Feature values look too good | Future events/properties leaked into a feature join | Compare feature timestamp with prediction timestamp for sampled rows |
| Row counts differ unexpectedly | Parse rejects, duplicate handling, or join multiplication | Reconcile counts at each pipeline stage and inspect key uniqueness |
| Re-run changes output | Unstable sorting/ties, random seed, unordered set/dict behavior, mutable inputs | Freeze seed and tie-break rules; compare intermediate artifacts |

## Recommendation/evaluation

| Symptom | Likely cause | First check |
|---|---|---|
| Recall is zero | Candidate generator excludes target, wrong target mapping, split issue, ID type mismatch | Trace one target from raw row through candidate IDs |
| Recall is implausibly high | Leakage, train/test overlap, target item in session state/features | Trace all inputs and timestamps for a few successful cases |
| Ranker does not beat baseline | Poor candidates, weak/noisy labels, invalid negatives, leakage-free features weak, not enough data | Measure candidate recall; inspect candidate-label rows before tuning model |
| Metric changes after filtering | Different eligible population or denominator | Record counts before and after every filter |
| Good global score, poor short-session result | Popularity dominates; model depends on long history | Stratify by history length and report fallback route |
| Coverage collapses | Popular items dominate or candidate sources are too narrow | Measure unique recommended items and source contribution |
| Train score high, validation low | Overfit, leakage in validation design, distribution shift | Compare time distributions and simplify features/model |

## Streaming and serving

| Symptom | Likely cause | First check |
|---|---|---|
| Event seems lost | Producer acknowledgment, consumer lag, wrong topic/key, or API acknowledgment misunderstood | Trace same event ID through API, stream, consumer, and state |
| Duplicate event changes recommendations twice | Missing idempotency or deduplication rule | Replay the same event ID twice and inspect state |
| Events arrive out of order | Partition key/consumer concurrency or late producer event | Log event time and processing time separately |
| API latency spikes | Redis/network/database calls, cold model load, serialization, or container resources | Measure per-stage timings and compare total |
| Local container won't start | Port conflict, missing env value, stale volume, resource limit | Read the first failing service log and validate its configuration |
| Same session returns different result | Nondeterministic tie-breaking, changing artifacts, or state race | Freeze inputs/model versions and inspect route/candidate order |

## Experiment and result integrity

- If a figure cannot be regenerated from saved config and data version, do not publish it as a result.
- If a run differs from the prior run, compare protocol, code version, seed, artifacts, and data.
- If a metric is undefined for a subset, report the subset count and rule; do not silently discard it.
- If an adaptive policy loses, report it. Do not tune against the test set.
- If latency is measured on one laptop under one workload, describe that context rather than generalizing.
- If replay injects synthetic events, label them as simulated scenarios.

## Self-review before release

Ask and answer in the README or experiment notes:

1. Can I explain every reported metric and denominator?
2. Can I trace one prediction from raw event through session, candidates, ranking, and output?
3. Can I show no future information entered that prediction?
4. Can I reproduce each result with recorded config and seed?
5. Did I compare against simple baselines on the same protocol?
6. Did I report negative results, failures, and limitations?
7. Can another person run the documented steps without hidden files or credentials?
8. Does every portfolio claim point to an artifact or measurement?

A “no” means document the gap or fix it before making the claim.
