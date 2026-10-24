# IntentFlow

**Adaptive Real-Time Session Recommendation Under Latency Constraints**

IntentFlow is a research-driven session recommendation project. It studies whether modeling an evolving session intent and adapting retrieval effort to a latency budget can improve next-item recommendations when long-term user histories are sparse or unavailable.

## Start here

1. [Project specification](docs/00-project-specification.md) — problem, research questions, focused V1, architecture, evaluation, and acceptance criteria.
2. [Self-study course](docs/learning/README.md) — lessons, exercises, and curated references.
3. [Self-guided manual](docs/04-self-guided-manual.md) — daily routine, eight-week schedule, decision gates, and independent phase reviews.
4. [Experiment workbook](docs/05-experiment-workbook.md) — worksheets, worked examples, and answer key.
5. [Implementation blueprint](docs/06-implementation-blueprint.md) — suggested repository structure, data contracts, service roles, and API shapes.
6. [Build plan](docs/03-build-plan.md) — phased task list and ownership split.
7. [Troubleshooting and self-review](docs/07-troubleshooting-and-self-review.md) — debugging checklist and release audit.
8. [Version history](docs/versions/README.md) — each major project direction in its own file.

## V1 constraints

- RetailRocket only; treat anonymous visitors, short sessions, sparse purchase events, implicit feedback, and hashed item properties as constraints to understand.
- Build from temporal data validation and sessionization through popularity/co-visitation baselines, retrieval, and a measured ranking experiment.
- Only then add the small online path: FastAPI, Redis session state, PostgreSQL metadata, Redpanda as local Kafka-compatible streaming, and Docker Compose.
- Use deterministic replay for offline policy comparisons and controlled robustness scenarios. Do not claim live-user A/B results.
- Learned ANN retrieval, neural ranking, and advanced intent models remain optional experiments.
- No experimental results are claimed until measured.

This repository contains the project plan and self-study material, not an implementation. You lead the learning, design decisions, coding, and experiment runs. The documents are designed to be usable independently; no mentor or chat check-in is required.
