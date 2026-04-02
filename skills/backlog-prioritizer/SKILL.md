---
name: backlog-prioritizer
version: 1.0.0
description: "Score and rank candidate features by impact, effort proxy, confidence, freshness, and evidence strength, while deduplicating overlapping ideas within and across repositories. Use after feature inference to turn a hypothesis list into a practical backlog."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [REPORT_TOP_FEATURES, ANALYSIS_MIN_ACTIVITY_THRESHOLD]
---
# Backlog Prioritizer

## What This Skill Does
Turn candidate features into a ranked backlog that a founder or engineering lead can act on immediately.

## Scoring Dimensions
Score every candidate on:
- impact: likely user or business upside
- effort_proxy: rough implementation complexity inferred from repo maturity and adjacent work
- confidence: how strongly the evidence supports the recommendation
- freshness: whether the signal is recent enough to matter now
- evidence_strength: breadth and quality of supporting artifacts

## Prioritization Rules
1. Deduplicate overlapping recommendations within a repository.
2. Deduplicate portfolio-level themes that are clearly the same opportunity across repositories.
3. Prefer candidates with strong, recent, multi-source evidence.
4. Penalize vague ideas with weak evidence.
5. Penalize stale signals if newer evidence contradicts them.
6. Separate quick wins from strategic bets.
7. Keep only the top `REPORT_TOP_FEATURES` recommendations in the primary user-facing list while preserving overflow internally if needed.

## Suggested Priority Bands
- P1: strong evidence, high impact, reasonable effort or urgent strategic gap
- P2: meaningful upside with moderate confidence or effort
- P3: interesting but weaker, speculative, or dependent on earlier work

## Output Contract
Return a ranked backlog with:
- final priority
- score breakdown
- deduplication notes
- repository coverage notes
- why higher-ranked items beat lower-ranked ones
