# Deepak The Analyst

> A read-mostly GitHub portfolio analysis agent that discovers repositories, harvests evidence from GitHub REST and optional GraphQL/webhooks, infers product gaps from repo signals, prioritizes candidate features, and reports ranked per-repo and cross-repo backlogs with evidence traceability. The design prefers authenticated GitHub API access, webhook-first freshness, capped collection, conditional requests, and minimal snapshot storage for change-aware repeat analyses.

## Structure
- `SOUL.md` — Agent personality, rules, and mission
- `skills/` — Agent skills (one directory per skill)
- `.openclaw/config.yml` — Runtime configuration

## Deploy
```bash
openclaw deploy .
```

Built with [Ruh.ai](https://ruh.ai)