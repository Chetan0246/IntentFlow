# IntentFlow Self-Study Course

This course teaches concepts needed to build IntentFlow, with exercises and primary references. It is paired with the [self-guided manual](../04-self-guided-manual.md), [experiment workbook](../05-experiment-workbook.md), [implementation blueprint](../06-implementation-blueprint.md), [build plan](../03-build-plan.md), and [troubleshooting guide](../07-troubleshooting-and-self-review.md). No mentor or chat check-in is required.

## Daily study routine

A suggested 60-minute session:

- **35 minutes — learn:** read one lesson section or primary source, taking brief notes.
- **10 minutes — recall:** close the source and explain the idea in your own words using a tiny example.
- **10 minutes — exercise/build:** solve a small exercise or complete one build ticket yourself.
- **5 minutes — journal:** record what you learned, evidence/output, confusion, and next action.

The **85% learning / 15% building** contract applies across the project. Early phases are mostly reading, reasoning, and small exercises; build time grows as understanding does.

## Lessons in order

1. [Recommender and evaluation foundations](01-recommender-foundations.md)
2. [RetailRocket data and sessionization](02-data-and-sessions.md)
3. [Baselines, retrieval, and ranking](03-baselines-retrieval-ranking.md)
4. [Intent, routing, and latency budgets](04-intent-and-adaptive-routing.md)
5. [Backend, streaming, and replay](05-backend-streaming-replay.md)
6. [Release and portfolio evidence](06-release-and-evidence.md)

Each lesson provides goals, explanations, exercises, build tickets, and study references. Complete its exercises before checking the worked examples in the experiment workbook.

## Weekly rhythm

Use five focused study days and one review/catch-up day, or adapt the rhythm to your schedule:

- Day 1: study a concept and make a note.
- Day 2: solve a toy example by hand.
- Day 3: inspect a small slice of the real dataset or code.
- Day 4: implement the smallest experiment yourself.
- Day 5: explain the result and its limits; self-review with the troubleshooting guide.
- Review day: revisit weak concepts and update the decision log.

## Learning-log template

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

## Source policy

Prefer the dataset owner's description, official project documentation, and original research papers. Treat blog posts and other repositories as examples, not authority. Check licenses before downloading or redistributing data. The RetailRocket Kaggle page lists **CC BY-NC-SA 4.0**; verify the current terms and follow them before sharing dataset files or derivatives.

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
