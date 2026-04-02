---
name: github-webhook-ingest
version: 1.0.0
description: "Validate GitHub webhook signatures, extract changed repository context from push, issues, pull_request, release, and repository events, and schedule incremental re-analysis for only affected repositories. Use when webhook-driven freshness is enabled."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash]
   env: [GITHUB_WEBHOOK_SECRET, GITHUB_OWNER, GITHUB_REPOSITORIES, GITHUB_EXCLUDED_REPOSITORIES]
---
# GitHub Webhook Ingest

## What This Skill Does
Handle GitHub webhook events safely and convert them into narrow re-analysis jobs.

## Supported Events
- push
- issues
- pull_request
- release
- repository

## Ingest Procedure
1. Verify the signature using `GITHUB_WEBHOOK_SECRET`.
2. Reject unsigned or invalid requests.
3. Parse event type and repository identity.
4. Confirm the repository is within the allowed scope after include and exclude filtering.
5. Extract the smallest useful changed context, such as:
   - pushed branch and commit range
   - issue action and labels
   - PR action, state, and merge status
   - release publication or edit
   - repository metadata changes
6. Schedule or trigger incremental re-analysis for only the affected repository or repository subset.
7. Prefer updating freshness and scores rather than re-running full portfolio scans unless the event implies broader impact.

## Security Rules
- Require HMAC signature validation.
- Do not trust webhook payloads without verification.
- Do not execute arbitrary payload content.

## Output Contract
Emit a normalized event summary with repository scope, triggering artifact, event type, and suggested follow-up analysis path.
