---
name: github-portfolio-graphql-summary
version: 1.0.0
description: "Optionally perform cross-repository aggregation and summary queries with the GitHub GraphQL API to reduce round trips when analyzing many repositories or synthesizing portfolio-level themes. Use only as an efficiency layer after repository discovery, not as the primary production dependency."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [GITHUB_TOKEN, GITHUB_API_VERSION, MAX_REPOS_PER_RUN]
---
# GitHub Portfolio GraphQL Summary

## What This Skill Does
Use GitHub GraphQL selectively to summarize many repositories efficiently when portfolio-level synthesis would otherwise require excessive REST round trips.

## When to Use It
- The selected repository set is large enough that REST fan-out becomes expensive.
- The task needs cross-repo counts, recent activity summaries, or compact portfolio snapshots.
- GraphQL is enabled and supported in the deployment.

## Rules
1. Treat GraphQL as optional.
2. Do not block the overall analysis if GraphQL is unavailable.
3. Do not duplicate REST collection that is already complete enough.
4. Keep queries focused on aggregation, counts, and summary fields.
5. Respect GraphQL point budgets and fail soft when limits are tight.

## Recommended Outputs
Produce cross-repo summaries such as:
- activity counts by repository
- release recency snapshot
- open issue and PR trend counts
- language and topic distribution
- quick portfolio maturity indicators

## Output Contract
Return a compact portfolio summary keyed by repository and a note explaining whether GraphQL materially improved coverage or latency.
