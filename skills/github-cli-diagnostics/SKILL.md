---
name: github-cli-diagnostics
version: 1.0.0
description: "Provide optional operational fallback and diagnostics using the GitHub CLI for local troubleshooting or validation, without serving as the primary production integration. Use when API behavior needs validation, auth must be checked locally, or a quick diagnostic comparison is helpful."
user-invocable: false
metadata:
 openclaw:
  requires:
   bins: [bash, gh]
   env: [GITHUB_TOKEN]
---
# GitHub CLI Diagnostics

## What This Skill Does
Use `gh` for troubleshooting and local validation when the primary API-driven workflow needs a sanity check.

## Appropriate Uses
- verify GitHub authentication locally
- compare API results against `gh` output
- inspect repository metadata or rate-limit behavior during debugging
- confirm access to a user, org, or repository

## Rules
- Treat `gh` as optional diagnostics only.
- Do not make it a required production dependency.
- Keep the workflow read-only.
- Use it to explain or validate collection failures, not to replace the main harvester.

## Diagnostic Checklist
1. Check whether `gh` is installed.
2. Check auth state using the configured token.
3. Validate access to the requested owner or repository.
4. Compare a small sample of repository metadata or issue listings against API results.
5. Report discrepancies and likely causes.
