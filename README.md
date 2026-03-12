# GetGrowth.eu — Agentic Content Engine

Automated ASO content pipeline that turns structured app audits into multi-channel content. Five AI agents orchestrated via n8n, backed by Supabase.

## Architecture

```
App Pool → Audit Agent → Teardown Writer → Repurposer → Publisher
                                                ↑
                              Benchmark Aggregator (monthly)
```

## Agents

| Agent | Schedule | Output |
|-------|----------|--------|
| **Audit Agent** | Weekly (Wed 8AM) | Structured ASO audit JSON |
| **Teardown Writer** | On audit | 1,500-word blog post |
| **Repurposer** | On blog | 2 LinkedIn posts + X thread + email snippet |
| **Benchmark Aggregator** | Monthly (1st) | Category benchmark article |
| **Publisher** | Daily (10AM M-F) | Pushes approved content to CMS + social |

## Stack

- **n8n** — workflow orchestration (5 workflows on deltabyetoro.app.n8n.cloud)
- **Supabase** — database (audits, content pieces, app pool, agent logs)
- **Gemini 2.0 Flash** — AI generation (audits, blog posts, social content)
- **Ghost/WordPress** — CMS publishing (configurable)
- **Buffer** — social media scheduling (LinkedIn, X)

## Quick Start

1. Set n8n environment variables (`SUPABASE_URL`, `SUPABASE_SERVICE_KEY`)
2. Seed `app_pool` table with public apps
3. Activate Audit Agent workflow, run manually once
4. Review drafts in `content_pieces`, mark as `approved`
5. Activate Publisher workflow

See [ARCHITECTURE.md](ARCHITECTURE.md) for full setup guide, schema docs, and runbook.

## Content Templates

- `templates/teardown-blog.md` — ASO teardown blog post structure
- `templates/linkedin-post.md` — LinkedIn post formats (3 variants)
- `templates/x-thread.md` — X/Twitter thread format (5-7 tweets)
- `templates/email-snippet.md` — Newsletter block
- `templates/benchmark-article.md` — Category benchmark report

## Audit Schema

`schemas/audit-output-schema.json` — the canonical structured output all agents consume.
