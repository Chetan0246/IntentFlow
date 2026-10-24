# Self-Guided IntentFlow Manual

This manual is written so you can study and build the project without a mentor or chat assistant. The lessons provide explanations and references; this document provides a schedule, work method, and completion gates. Use the specification as the source of truth.

## What this repo gives you—and what you must do

The repository contains the project definition, a learning course, exercises, the implementation plan, and this manual. It intentionally does not contain the implementation or results. You will obtain the dataset under its license, make the documented decisions, write and run the system, and record real outputs.

Do not expect a recommendation to improve. A correct experiment can show no improvement or a regression; an honest explanation is a successful result.

## One study session

Use 60–90 minutes as available:

1. 5 minutes: read the phase goal and today's deliverable.
2. 25–35 minutes: study one concept from its lesson and primary reference.
3. 10 minutes: close the source and explain it from memory.
4. 15–30 minutes: solve the exercise or implement one small task yourself.
5. 5–10 minutes: update the learning log and write the next action.

If you have only 30 minutes, study one concept and write one paragraph. Resume next day; do not compensate by pasting a large codebase.

## Eight-week guided schedule

The schedule assumes five focused sessions each week. It can take longer. The “build” column is a small learner-owned artifact, not a requirement to code every day.

| Week | Study | Build/produce | Week-end gate |
|---|---|---|---|
| 1. Define task | Lesson 1: implicit feedback, next-item prediction, retrieval vs. ranking, metrics | Protocol worksheet; hand-computed metric examples | You can explain what is visible at a prediction point and how target leakage occurs |
| 2. Understand data | Lesson 2: RetailRocket files, event semantics, timestamps, data quality, license | Data dictionary and profile report after obtaining data | Actual file schema and quality observations are documented; raw files remain unchanged |
| 3. Sessions and split | Lesson 2: session gaps, time ordering, temporal split, point-in-time joins | Sessionizer and split manifest | Toy cases and real samples follow the written rule; split uses chronology |
| 4. Baselines | Lesson 3: popularity, co-visitation, candidate recall and evaluation | Popularity and co-visitation systems plus metric report | Both baselines use the same eligible prediction points and training cutoff |
| 5. Candidate ranking | Lesson 3: feature timestamps, negatives, ranker objectives, ablations | Candidate table, optional LightGBM ranker, ablation | You can trace one ranked candidate's features and labels to their source |
| 6. Intent and router | Lesson 4: proxies, state, confidence, routing, cost | Interpretable state, route logs, fixed/adaptive comparison | Route decisions can be explained; unsupported intent interpretations are excluded |
| 7. Online path and replay | Lesson 5: API, state store, event ordering, idempotency, deterministic replay | Pure in-memory core, API, then Redis/Redpanda where justified | Replay produces a deterministic trace and hides the target until scoring |
| 8. Evidence and release | Lesson 6: latency, failures, reproducibility, limits | Demo, measured load run, result table, failure analysis, README | Another person can follow the instructions; claims match stored evidence |

Weeks 7–8 may expand to weeks 9–10 if infrastructure takes longer. When behind, defer ANN, two-tower models, neural rankers, learned intent, and UI polish before compromising leakage controls, baseline comparability, or documentation.

## Daily sequence for the first 10 sessions

Use the detailed exercises in [the experiment workbook](05-experiment-workbook.md).

1. Mark visible history and next-item target in a five-event sequence.
2. Explain implicit feedback and what views/carts/transactions do and do not mean.
3. Draw retrieval → ranking → post-filtering; name a failure for each step.
4. Hand-calculate Recall@K and reciprocal rank.
5. Explain random-split leakage; write a chronological split plan.
6. Read the dataset card; list files, fields, event types, time-varying properties, and license.
7. Create a data-profile plan; then run it on the downloaded files.
8. Sessionize a tiny timeline under two gap thresholds.
9. Hand-calculate popularity and co-visitation for tiny sessions.
10. Fill the experiment protocol, self-check it, and only then begin model code.

## Decision gates before building

Complete these in writing. Candidate defaults are examples to investigate, not hidden ground truth.

1. **Target:** next event item, next distinct item, or another target? State event types, repeat-item policy, and eligible prediction points.
2. **Session:** choose inactivity threshold and timestamp tie policy. Compare a plausible alternative and show how assignments change.
3. **Split:** choose chronological boundaries after inspecting timestamps. Save them in a manifest.
4. **Candidates:** define catalogue universe, seen-item rule, candidate count, and zero-candidate handling.
5. **Metrics:** specify aggregation, denominators, filtering, and metric definitions.
6. **Features:** list each feature and its latest allowed timestamp.
7. **Robustness:** define each synthetic perturbation precisely; keep clean replay as control.
8. **Latency:** record machine, software, workload, concurrency, warm-up, sample count, and measurement boundary before reporting.

Use [the protocol worksheet](05-experiment-workbook.md#protocol-worksheet) to record each decision and reason.

## Solo review at every phase

Before proceeding, answer without looking at notes:

- What problem did I solve?
- Why did I choose this approach?
- What evidence supports the choice?
- What is one alternative and what would it cost?
- What could leak future information?
- What failed or remains uncertain?
- Can I reproduce the output from a clean run?

If an answer is unclear, return to the corresponding lesson and run a smaller exercise. A passing phase gate means you can explain it and reproduce it, not merely that code executes.

## Weekly review

At the end of each week:

- Write a 5–10 line summary in your own words.
- Add actual data/experiment evidence or clearly mark it pending.
- Update the decision log when a choice changes; keep the old reasoning rather than silently replacing it.
- Check the next phase prerequisites.
- Choose one small task for the next study day.

## Security and data handling

Keep credentials in local environment variables or a local ignored file; never commit credentials. Keep dataset files local unless the license and repository policy allow distribution. The dataset card currently lists CC BY-NC-SA 4.0; verify the current terms before sharing data or derivatives. Do not expose private data in logs or screenshots.

## Final self-certification

Before calling the project done, confirm the acceptance list in the project specification and ask yourself:

- Are all result values backed by a saved run?
- Can someone regenerate the results?
- Are offline replay and causal/online claims clearly distinguished?
- Can I explain every component and why it exists?
- Are failures and the adaptive hypothesis outcome represented honestly?
