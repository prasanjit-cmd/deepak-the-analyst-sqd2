---
name: feature-gap-analyzer
version: 1.0.0
description: "Transform normalized repository evidence into candidate feature hypotheses using heuristics such as repeated user requests, recurring bug classes, missing integrations, weak onboarding/docs, stale roadmap items, missing observability, and unaddressed TODO/FIXME clusters. Use after evidence harvesting to infer what should likely be built next."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [FEATURE_LOOKBACK_DAYS, ANALYSIS_MIN_ACTIVITY_THRESHOLD]
---
# Feature Gap Analyzer

## What This Skill Does
Convert repository evidence into candidate product opportunities without hallucinating certainty.

## Feature-Inference Heuristics
Look for these evidence patterns:
- repeated issue requests that imply unmet demand
- recurring bug classes that suggest missing product surface or automation
- multiple PRs or commits around the same pain point
- weak onboarding or missing setup docs that suggest guided setup, starter templates, or better DX
- missing integrations repeatedly mentioned in docs, issues, or commits
- release cadence drift showing roadmap promises not landing
- TODO/FIXME clusters implying unfinished capabilities
- absent observability, admin tooling, or monetization hooks in otherwise mature repositories
- repeated labels like enhancement, help wanted, bug, onboarding, or docs that cluster around the same gap

## Analysis Procedure
1. Group evidence by repository.
2. Cluster related issues, PRs, commits, labels, docs notes, and release signals into themes.
3. Translate each theme into a candidate feature phrased as a concrete backlog item.
4. Attach supporting evidence references.
5. Mark weak or sparse themes as low confidence.
6. If a repository has fewer than `ANALYSIS_MIN_ACTIVITY_THRESHOLD` meaningful signals, still analyze it but downgrade confidence and narrow claims.

## Candidate Feature Format
For each candidate, produce:
- title
- repository or repo cluster
- problem statement
- inferred user value
- key evidence references
- why now
- confidence basis

## Guardrails
- Do not recommend features that are already clearly shipped in recent releases unless the evidence says adoption or completion is still missing.
- Do not confuse maintenance chores with customer-facing features unless the maintenance gap directly blocks product capability.
- Avoid duplicate candidates that differ only in wording.
- Prefer actionable backlog wording over abstract product advice.
