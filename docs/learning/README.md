# IntentFlow Self-Study Course

This is the course to use alongside the project specification. It teaches the concepts needed to build IntentFlow, gives you small exercises, and points to primary learning material. Work through it in order; do not copy a finished recommender into the repository.

## How to study each day

A suggested 60-minute session:

- **35 minutes — learn:** read one section or source, taking brief notes.
- **10 minutes — retrieve from memory:** close the source and explain the idea in your own words, with a tiny example.
- **10 minutes — exercise/build:** solve the day's small exercise or complete one build ticket yourself.
- **5 minutes — journal:** write what you learned, what you tried, evidence, confusion, and next action.

This is a daily rhythm, not a stopwatch. The **85% learning / 15% building** contract applies across the project: early work is mostly reading, reasoning, and tiny experiments; the implementation share grows only as understanding grows. Learning notes are real deliverables.

## Recommended order

1. [Recommender and evaluation foundations](01-recommender-foundations.md)
2. [RetailRocket data and sessionization](02-data-and-sessions.md)
3. [Baselines, retrieval, and ranking](03-baselines-retrieval-ranking.md)
4. [Intent, routing, and latency budgets](04-intent-and-adaptive-routing.md)
5. [Backend, streaming, and replay](05-backend-streaming-replay.md)
6. [Release and portfolio evidence](06-release-and-evidence.md)

Read the matching project task in [the build plan](../03-build-plan.md) after the lesson, then do the task yourself.

## Weekly cadence

Use five study days and one review/catch-up day. Keep one day free if that helps you stay consistent.

- Day 1: learn a concept and make a one-page note.
- Day 2: work through a toy example by hand.
- Day 3: inspect a small slice of the real dataset or code.
- Day 4: implement the smallest experiment yourself.
- Day 5: explain the result and its limitations; ask for code review.
- Review day: revisit weak concepts and update the decision log.

## Learning log template

Copy this for each note:

```markdown
# Topic

## Question I wanted to answer
## What I thought before studying
## Explanation in my own words
## Tiny example / exercise
## Decision for IntentFlow and alternatives
## Evidence or output (leave pending until measured)
## What failed, surprised me, or remains uncertain
## Next step
## Sources read
```

## Pairing contract

You own the understanding, design decisions, implementation, and experiment runs. Codex teaches, provides exercises, helps you reason about trade-offs, reviews code you wrote, and interprets actual outputs with you. Start by asking for a lesson and hints; ask for full code only if you explicitly want a reference implementation after attempting the task.

## Source policy

Prefer the dataset owner's description, official project documentation, and original research papers. Treat blog posts and other repositories as examples, not authority. Check licenses before downloading or redistributing data. RetailRocket's Kaggle dataset page lists **CC BY-NC-SA 4.0**; verify the current terms and follow them before sharing dataset files or derived assets.

## Curated source shelf

- [RetailRocket dataset card on Kaggle](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset)
- [Google ML: recommendation system overview](https://developers.google.com/machine-learning/recommendation/overview)
- [Google ML: candidate generation](https://developers.google.com/machine-learning/recommendation/overview/candidate-generation)
- [Google ML: ranking](https://developers.google.com/machine-learning/recommendation/overview/ranking)
- [Ludewig & Jannach, Evaluation of Session-based Recommendation Algorithms](https://arxiv.org/abs/1803.09587)
- [pandas getting started](https://pandas.pydata.org/docs/getting_started/index.html)
- [DuckDB Python API](https://duckdb.org/docs/stable/clients/python/overview)
- [LightGBM documentation](https://lightgbm.readthedocs.io/en/stable/)
- [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/)
- [Redis Python guide](https://redis.io/docs/latest/develop/clients/redis-py/)
- [Apache Kafka documentation](https://kafka.apache.org/documentation/)
- [Redpanda quickstart](https://docs.redpanda.com/current/get-started/quick-start/)
- [Docker Compose quickstart](https://docs.docker.com/compose/gettingstarted/)
- [Locust documentation](https://docs.locust.io/en/stable/)
- [Google ML: fairness and recommendation](https://developers.google.com/machine-learning/recommendation)
