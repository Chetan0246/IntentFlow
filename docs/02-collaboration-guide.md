# How We’ll Work on IntentFlow

You are the learner and primary builder. Codex is your tutor, planning partner, and reviewer. The **85% learning / 15% building** contract means the implementation should remain yours: Codex explains, asks you to reason through choices, helps design small experiments, reviews your code, and debugs with you. It should not silently build the whole system for you.

## Responsibility split

| Work | You own | Codex supports with |
|---|---|---|
| Learning goals | Choose what you want to understand; explain concepts back in your own words | Clear explanations, examples, reading questions, and a small practice exercise |
| Research protocol | Decide sessionization, split, metrics, candidate policy, and assumptions after understanding trade-offs | Enumerate alternatives, identify leakage risks, critique protocol, and help document decisions |
| Implementation | Write and run the code yourself; make the final technical choices | Give bounded hints, pseudocode when requested, code review, debugging, and explanations of errors |
| Experiments | Run the experiments; preserve logs and actual outputs | Help design fair comparisons, interpret outputs, and distinguish evidence from assumptions |
| Documentation | Keep a learning journal and record decisions, failures, and results | Edit/review prose with you; never fabricate results |
| Scope decisions | Decide whether an optional feature earns its place | Explain costs and help compare against the project question |
| Final portfolio claims | Verify every claim against artifacts and measurements | Audit claims for accuracy, clarity, and reproducibility |

## Session workflow

For each phase:

1. **You state your learning goal** and what you currently think the answer is.
2. **Codex teaches and probes:** explanation, examples, and a small check-your-understanding question.
3. **You design the experiment/implementation** in your own words; Codex critiques it and points out likely failure modes.
4. **You build and run it.** If blocked, share the smallest relevant code/error context.
5. **Codex reviews; you make the changes** and explain the important one.
6. **You record measured evidence yourself** (Codex can help interpret actual output) and update the learning note.
7. Move on only when the phase exit evidence in the roadmap is met.

## Guardrails

- Ask before generating a full implementation when the goal is learning; default to explanation, tiny examples, or hints.
- Never fabricate metrics, benchmarks, data findings, citations, or experiment outcomes.
- Keep optional technologies optional. Every addition needs a named learning goal and an experiment that can show its value.
- Prefer a small, working, understood slice over a large pasted system.
- Codex may prepare documentation/scaffolding and perform a requested review, but you remain the author of system behavior and can explain each component.
- No external messages, deployment, or changes to repository privacy/settings are implied by this collaboration plan.

## Suggested first working session

Start Phase 0. Ask Codex to teach temporal leakage in next-item recommendation using a tiny hand-worked event sequence. Then, in your own words, propose a prediction point, what history is visible, and how the target is held out. Have Codex critique that protocol before touching the dataset.
