# Version 3 — IntentFlow: Initial Differentiation

**Earlier subtitle:** Adaptive Real-Time Session Recommendation & Behavioral Intelligence Engine  
**Status:** Historical framing, refined by Version 4

## Project idea

Go beyond “recommend the next product.” Detect how a visitor's intent may be evolving during a session, adapt recommendation strategy to that state, and evaluate quality, robustness, and latency.

## Three signature ideas

1. **Intent trajectory:** model an evolving state rather than a bare event list. Exploring → narrowing → comparing → committing was proposed as an interpretable conceptual trajectory, not supplied ground truth.
2. **Adaptive recommendation:** use different paths for short/uncertain and richer sessions.
3. **Robustness and latency:** study behavior under noisy interactions and system constraints.

## Candidate approach

Popularity is a fallback; co-visitation is a session-oriented retrieval method; embedding/ANN retrieval is an optional advanced path. LightGBM is the initial ranker, with neural ranking treated as an experiment rather than a requirement.

## Evaluation ideas

Compare fixed and adaptive policies using Recall@20, NDCG@10, MRR, coverage, latency percentiles, robustness cases, route/fallback rates, and quality-versus-cost. Build an inspectable failure analysis view.

## Refinement

The final plan made the research question more precise: does evolving session intent justify different computational effort under a latency budget? It also separated offline learning, online serving, and deterministic replay into three logical planes and clarified a minimal acceptance protocol.

No results or validated psychological interpretation were established in this version.
