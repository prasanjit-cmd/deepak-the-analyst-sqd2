---
name: github-repo-discovery
version: 1.0.0
description: "Resolve the target GitHub owner or organization, inventory candidate repositories, apply include or exclude filters, and select active repositories for analysis using repository metadata such as stars, language mix, archived state, fork state, topics, release presence, and recent push activity. Use when a GitHub feature-backlog run begins or when scope needs to be narrowed before evidence collection."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [GITHUB_TOKEN, GITHUB_OWNER, GITHUB_REPOSITORIES, GITHUB_EXCLUDED_REPOSITORIES, MAX_REPOS_PER_RUN, CACHE_TTL_MINUTES, GITHUB_API_VERSION]
---
# GitHub Repo Discovery

## What This Skill Does
Resolve the analysis target, discover candidate repositories, enforce repository filters, and choose the active repositories worth deeper analysis.

## Inputs
- Explicit owner, organization, or repository list from the user request
- Fallback owner from `GITHUB_OWNER`
- Optional allowlist from `GITHUB_REPOSITORIES`
- Optional blocklist from `GITHUB_EXCLUDED_REPOSITORIES`
- Run cap from `MAX_REPOS_PER_RUN`
- Cache horizon from `CACHE_TTL_MINUTES`

## Discovery Rules
1. Prefer explicit user scope over environment defaults.
2. Treat the scope as one of:
   - a user account
   - an organization
   - a selected repository set
3. Fetch repository inventory through GitHub REST.
4. Collect, at minimum, for each repository:
   - name and full name
   - visibility if accessible
   - description
   - primary language and language mix
   - stars, forks, watchers if available
   - topics
   - default branch
   - archived state
   - fork state
   - open issues count
   - pushed-at and updated-at
   - release presence
5. Drop repositories excluded by blocklist.
6. If an allowlist is present, analyze only those repositories.
7. Exclude archived repositories and obvious forks from the default active set unless the user explicitly asks to include them.
8. Rank remaining repositories by recent push activity, issue/PR activity, release activity, and general product relevance.
9. Enforce `MAX_REPOS_PER_RUN`.

## Output Contract
Produce a normalized repository inventory containing:
- target owner or org
- selected repositories
- repositories skipped and why
- activity summary per selected repository
- coverage notes if auth or visibility limits block access

## Rate and Cache Guidance
- Use authenticated requests with `Authorization: Bearer`.
- Send `Accept: application/vnd.github+json`.
- Send `X-GitHub-Api-Version` from `GITHUB_API_VERSION`.
- Reuse fresh cached inventory when within `CACHE_TTL_MINUTES`.
- Follow pagination through `Link` headers.
- Prefer conditional GETs with `ETag` or `If-Modified-Since` when possible.

## Failure Handling
- If owner resolution fails, report the missing or invalid scope.
- If no repositories survive filtering, say so clearly rather than proceeding with empty analysis.
- If access is partial, continue with visible repositories and annotate the gap.
