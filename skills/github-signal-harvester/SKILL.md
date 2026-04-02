---
name: github-signal-harvester
version: 1.0.0
description: "Collect normalized evidence from each selected GitHub repository, including issues, pull requests, commits, releases, labels, languages, README and selected docs content, and optional search-derived indicators such as TODO/FIXME clusters and roadmap hints. Use after repository discovery when building the evidence base for feature inference."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [GITHUB_TOKEN, GITHUB_API_VERSION, FEATURE_LOOKBACK_DAYS, MAX_ISSUES_PER_REPO, MAX_PRS_PER_REPO, MAX_COMMITS_PER_REPO, CACHE_TTL_MINUTES]
---
# GitHub Signal Harvester

## What This Skill Does
Harvest the raw GitHub evidence needed to infer product opportunities without mutating repository state.

## Evidence to Collect Per Repository
- Repository metadata refresh
- Issues: open and recently closed, including labels and titles
- Pull requests: merged, open, and recently closed themes
- Commit history within `FEATURE_LOOKBACK_DAYS`
- Releases and tags relevant to product cadence
- Labels and issue taxonomy
- Languages and repository composition
- README and selected docs such as roadmap, contributing, changelog, docs index, setup, onboarding, and integration pages when present
- Optional code-search indicators for TODO/FIXME and roadmap hints when enabled

## Harvesting Procedure
1. Start from the selected repository list from discovery.
2. For each repository, fetch issues up to `MAX_ISSUES_PER_REPO`.
3. Fetch pull requests up to `MAX_PRS_PER_REPO`.
4. Fetch commits up to `MAX_COMMITS_PER_REPO`, constrained by `FEATURE_LOOKBACK_DAYS` where practical.
5. Fetch releases and release notes.
6. Fetch repository languages and key top-level docs.
7. If code search is enabled, perform tightly-scoped searches for TODO, FIXME, roadmap, backlog, planned, and similar terms.
8. Normalize all artifacts into compact evidence records tagged by repository, artifact type, timestamp, and relevance.

## Normalized Evidence Model
Each evidence record should capture:
- repository
- artifact_type
- artifact_id or stable reference
- title or summary
- state
- labels or tags
- author or actor if relevant
- created_at / updated_at / merged_at / published_at as applicable
- short evidence snippet
- evidence_url or API path
- freshness bucket

## Collection Heuristics
- Favor titles, labels, and summaries over full-body dumping when rate or token budgets matter.
- Pull enough content to justify an inference, not entire private codebases.
- Read README and product-facing docs for onboarding, positioning, integration, and roadmap gaps.
- Treat repeated bug reports, recurring support labels, and implementation churn as feature signals.

## Rate-Limit Handling
- Respect `X-RateLimit-*` headers.
- For secondary rate limits, back off for at least 60 seconds, then retry with exponential backoff.
- If `Retry-After` is present, wait at least that long.
- Cap code-search usage carefully because authenticated code search is especially limited.

## Output Contract
Return a repository-indexed evidence bundle plus collection notes, including skipped endpoints, cache hits, and confidence-affecting gaps.
