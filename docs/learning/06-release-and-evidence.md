# Lesson 6 — Release, Failure Analysis, and Evidence

## Learning goals

Turn implementation into a reproducible portfolio artifact without overstating results. Learn to explain what worked, what failed, and what the experiment can support.

## Reproducibility

A project is reproducible when another person can obtain the allowed data, follow exact environment/setup instructions, run the pipeline, and regenerate the documented outputs. Record dependency versions, random seeds, data source/version, split boundaries, configuration, and commands. Avoid committing secrets, large raw data, or machine-specific absolute paths.

## Results tables

Keep a result table with fixed rows for popularity, co-visitation, best fixed policy, and adaptive policy. Columns may include Recall@20, NDCG@10, MRR, coverage, p95 latency, route share, and fallback rate. Fill cells only from captured runs. If a run has not happened, write “pending”; if invalid, explain why rather than hiding it.

Record the evaluation protocol next to the table: target definition, eligible examples, candidate universe, filtering, split dates, hardware, workload, and latency method.

## Failure analysis

Choose representative failures from actual replay output. Show:
- session prefix visible at prediction time;
- expected next event (revealed only for evaluation);
- returned recommendations and candidate sources;
- intent features and selected route;
- error/failure category and possible cause.

Do not cherry-pick one compelling example as evidence of general improvement. Pair case studies with aggregate results and limitations.

## Portfolio claims

Use precise claims: “measured on a temporal holdout” rather than “works in production”; “offline replay” rather than “A/B test”; “interpretable intent proxy” rather than “understands users.” Explain the one-dataset limitation and the distinction between prediction accuracy and causal impact.

## Exercises

1. Draft a results table with all planned rows/columns and no values.
2. Write a reproduction checklist someone else can follow.
3. Select one bad recommendation from real output and explain what information the system had.
4. Write a two-sentence conclusion for an imagined negative result without changing the facts.
5. Audit a proposed resume bullet: highlight each claim that needs an artifact or measurement.

## Build tickets

- **P1:** Add a one-command documented local workflow after implementation stabilizes.
- **P2:** Save experiment configs and logs alongside reported results.
- **P3:** Create Failure Lab summaries from actual replay traces.
- **P4:** Write README results, design trade-offs, limitations, and failures.
- **P5:** Re-run the documented workflow in a clean environment before claiming reproducibility.

## Study material

- [Docker Compose quickstart](https://docs.docker.com/compose/gettingstarted/)
- [Ludewig & Jannach session-based evaluation paper](https://arxiv.org/abs/1803.09587)
- [Google ML recommendation overview](https://developers.google.com/machine-learning/recommendation/overview)
