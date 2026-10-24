# IntentFlow

**Adaptive Real-Time Session Recommendation Under Latency Constraints**

IntentFlow is a research-driven session recommendation project. It studies whether modeling an evolving session intent and adapting retrieval effort to a latency budget can improve next-item recommendations when long-term user histories are sparse or unavailable.

## Start here

1. [Project specification](docs/00-project-specification.md) — problem, research questions, focused V1, architecture, evaluation, and acceptance criteria.
2. [Learning roadmap](docs/01-learning-roadmap.md) — staged 85% learning / 15% building plan.
3. [Collaboration guide](docs/02-collaboration-guide.md) — what you own and how Codex supports without taking the learning away.
4. [Version history](docs/versions/README.md) — each major project direction in its own file.

## V1 constraints

- RetailRocket only; treat anonymous visitors, short sessions, sparse purchase events, implicit feedback, and hashed item properties as constraints to understand.
- Build from temporal data validation and sessionization through popularity/co-visitation baselines, retrieval, and a measured ranking experiment.
- Only then add the small online path: FastAPI, Redis session state, PostgreSQL metadata, Redpanda as local Kafka-compatible streaming, and Docker Compose.
- Use deterministic replay for offline policy comparisons and controlled robustness scenarios. Do not claim live-user A/B results.
- Learned ANN retrieval, neural ranking, and advanced intent models remain optional experiments.
- No experimental results are claimed until measured.

This repository starts with documentation only. Learn the concepts, run small experiments, and implement the system yourself; use Codex as a coach and reviewer.
