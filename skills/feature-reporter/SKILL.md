---
name: feature-reporter
version: 1.0.0
description: "Format the final output into a structured per-repo and portfolio-level ranked backlog with rationale, evidence references, priority, confidence, and suggested next steps, including change-aware refresh summaries when prior snapshots exist. Use at the end of the GitHub analysis workflow to produce the user-facing report."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [REPORT_TOP_FEATURES]
---
# Feature Reporter

## What This Skill Does
Turn the ranked backlog into a report that is easy to scan, easy to trust, and easy to act on.

## Report Structure
1. Scope summary
   - owner or org analyzed
   - repositories included and skipped
   - coverage limitations
2. Portfolio summary
   - top cross-repo opportunities
   - overall maturity and activity notes
3. Per-repository opportunities
   - ranked feature list for each repository
4. Change summary
   - what changed since the last analysis
   - newly supported ideas
   - dropped or downgraded ideas
5. Recommended next steps
   - immediate validation or implementation suggestions

## Formatting Rules
- Lead with the highest-value recommendations.
- Keep titles concrete.
- Cite evidence inline using repository and artifact references.
- Distinguish evidence from inference.
- Make confidence visible.
- Avoid walls of prose when bullets or compact sections are clearer.

## Required Fields Per Recommendation
- feature title
- repository or portfolio scope
- priority
- rationale
- evidence references
- confidence
- suggested next step

## Change-Aware Reporting
When prior snapshots exist:
- compare current recommendations to the last stored run
- highlight net-new opportunities
- mark priority shifts
- identify evidence that weakened or strengthened a recommendation
- avoid repeating unchanged boilerplate unless the user asks for the full report
