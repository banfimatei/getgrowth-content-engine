# GetGrowth.eu Agentic Content Engine — Architecture & Runbook

## Overview

A fully automated content pipeline that treats each ASO audit as structured data and generates multi-channel content from it. Five AI agents orchestrated via n8n, backed by Supabase for storage and state management.

## System Architecture

```
                            ┌─────────────────────────┐
                            │     App Pool (Supabase)  │
                            │  Curated public apps by  │
                            │  category/region/size    │
                            └───────────┬─────────────┘
                                        │
                            ┌───────────▼─────────────┐
                            │  1. AUDIT AGENT          │
                            │  Wed 8AM • 3 apps/week   │
                            │  iTunes API → Gemini →   │
                            │  Structured audit JSON   │
                            └───────────┬─────────────┘
                                        │ triggers
                            ┌───────────▼─────────────┐
                            │  2. TEARDOWN WRITER      │
                            │  Audit JSON → 1,500-word │
                            │  blog post (markdown)    │
                            └───────────┬─────────────┘
                                        │ triggers
                            ┌───────────▼─────────────┐
                            │  3. REPURPOSER AGENT     │
                            │  Blog → 2 LinkedIn posts │
                            │       → 5-tweet thread   │
                            │       → email snippet    │
                            └───────────┬─────────────┘
                                        │
         ┌──────────────────────────────┤
         │                              │
┌────────▼────────┐          ┌──────────▼──────────┐
│ 4. BENCHMARK    │          │  5. PUBLISHER AGENT  │
│ AGGREGATOR      │          │  Daily 10AM (M-F)    │
│ Monthly 1st     │          │  Approved → CMS/     │
│ 5+ audits/cat   │          │  Buffer/Social APIs  │
│ → category      │          └──────────────────────┘
│   benchmark     │
│   articles      │
└─────────────────┘
```

## n8n Workflow IDs

| # | Workflow | n8n ID | Schedule | Trigger |
|---|---------|--------|----------|---------|
| 1 | App Selector + Audit Agent | `UAlNy3H5lwfeW4tA` | Wed 8AM | Cron + Manual |
| 2 | Teardown Writer Agent | `teCGW1ZLKvxcmGho` | On demand | Execute Workflow + Webhook |
| 3 | Repurposer Agent | `pVqwJTNZPrj1BTSC` | On demand | Execute Workflow |
| 4 | Benchmark Aggregator Agent | `8PpycXxQthfXmWaw` | 1st of month, 9AM | Cron + Manual |
| 5 | Publisher Agent | `hk5AFq3I4Gd9EPpO` | Daily 10AM (M-F) | Cron + Manual |

## Supabase Schema

### Existing Tables (getgrowth.eu core)
- `users` — registered users (Clerk ID, Stripe, audit credits)
- `saved_apps` — apps saved by users
- `audits` — ASO audit results (extended with content pipeline fields)
- `competitors` — competitor apps per saved app
- `ai_unlocks` — AI feature unlocks
- `purchases` — Stripe purchases

### New Tables (content pipeline)
- `content_pieces` — every generated asset (blog, LinkedIn, X, email, benchmark)
- `content_queue` — scheduling and publishing pipeline
- `benchmark_reports` — aggregated category-level benchmark reports
- `app_pool` — curated public apps for automatic auditing
- `content_agent_log` — tracks every agent run for monitoring

### Audit Table Extensions
Added columns to `audits`:
- `findings` (jsonb) — structured top issues array
- `recommendations` (jsonb) — prioritized recs array
- `experiment_ideas` (jsonb) — experiment hypothesis array
- `app_category`, `app_store_url`, `country_code`
- `visibility_score`, `cvr_score`
- `content_generated` (bool) — flag for teardown writer
- `source` — 'manual', 'auto_public', 'free_audit', 'paid_audit'

## Content Lifecycle

```
AUDIT → DRAFT → REVIEW → APPROVED → PUBLISHED
  │                                      │
  │  (Agents generate content)           │ (Publisher pushes to channels)
  │                                      │
  └──────── content_pieces.status ───────┘
```

### Status Flow
1. **draft** — AI-generated, not yet reviewed
2. **review** — flagged for human review (optional QA step)
3. **approved** — ready for publishing (can be auto-set or manual)
4. **published** — pushed to CMS/social via Publisher Agent
5. **rejected** — won't be published

### Auto-Approve (Optional)
To run fully hands-off, add a Code node after content generation that auto-sets status to 'approved'. The QA step is the human-in-the-loop toggle point.

## Environment Variables Required

Set these in n8n (Settings → Variables):

```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=eyJ... (service role key)

# CMS (Ghost, WordPress, Webflow)
CMS_API_URL=https://your-blog.ghost.io/ghost/api/admin
CMS_API_KEY=your-ghost-admin-api-key
CMS_TYPE=ghost

# Buffer (social media scheduling)
BUFFER_ACCESS_TOKEN=your-buffer-token
BUFFER_LINKEDIN_PROFILE=your-linkedin-profile-id
BUFFER_X_PROFILE=your-x-profile-id
```

## Seeding the App Pool

Before the first run, populate the `app_pool` table with public apps to audit:

```sql
INSERT INTO app_pool (store_id, platform, name, category, country_code, priority) VALUES
  ('com.headspace.headspace', 'ios', 'Headspace', 'Health & Fitness', 'US', 5),
  ('com.calm.ios', 'ios', 'Calm', 'Health & Fitness', 'US', 5),
  ('com.duolingo', 'ios', 'Duolingo', 'Education', 'US', 4),
  ('com.spotify.client', 'ios', 'Spotify', 'Music', 'US', 3),
  ('com.nianticlabs.pokemongo', 'ios', 'Pokémon GO', 'Games', 'US', 3),
  ('com.strava', 'ios', 'Strava', 'Health & Fitness', 'US', 4),
  ('com.revolut.revolut', 'ios', 'Revolut', 'Finance', 'GB', 5),
  ('com.n26', 'ios', 'N26', 'Finance', 'DE', 4),
  ('com.waze', 'ios', 'Waze', 'Navigation', 'US', 3),
  ('com.todoist', 'ios', 'Todoist', 'Productivity', 'US', 3);
```

Higher `priority` = audited first. `last_audited_at` NULL = never audited (top priority).

## Content Templates

Located in `getgrowth-content-engine/templates/`:

| Template | Agent | Output |
|----------|-------|--------|
| `teardown-blog.md` | Teardown Writer | 1,200-1,800 word blog post |
| `linkedin-post.md` | Repurposer | 2-3 LinkedIn posts per audit |
| `x-thread.md` | Repurposer | 5-7 tweet thread |
| `email-snippet.md` | Repurposer | Newsletter paragraph |
| `benchmark-article.md` | Benchmark Aggregator | 2,000-3,000 word category report |

## Operational Runbook

### First-Time Setup
1. Set environment variables in n8n
2. Seed app_pool with 10-20 apps across 3-5 categories
3. Activate Workflow 1 (Audit Agent) and Workflow 5 (Publisher)
4. Run Workflow 1 manually once to verify end-to-end
5. Review draft content in Supabase `content_pieces` table
6. Mark as 'approved' to trigger publishing

### Weekly Routine
- **Your only job**: skim draft content, approve or reject
- Optionally: add new apps to app_pool
- Optionally: tweak prompts in templates if tone drifts

### Monthly
- Workflow 4 runs automatically on the 1st
- Review benchmark articles (usually higher quality, less editing needed)
- Add new categories to app_pool as you expand

### Monitoring
- Check `content_agent_log` for failed runs
- n8n execution history shows detailed logs per workflow
- Supabase dashboard → content_pieces table for content inventory

### Scaling
- Increase `APPS_PER_WEEK` in Workflow 1's Set Config node
- Add more categories/regions to app_pool
- Add BTTS Market content types to the content_type enum if needed
- Swap Ghost CMS for Webflow/WordPress by changing Publisher Agent URLs

## Handling "No Clients" Gracefully

All auto-generated content uses public App Store data:
- "We're running public ASO teardowns of real apps to showcase our audit tool"
- No client permission needed for public store listings
- Later: offer free audits in exchange for permission to publish named results
- Gradually layer in real case studies as clients appear

## Audit JSON Schema

Canonical schema at `getgrowth-content-engine/schemas/audit-output-schema.json`.

Key fields every content agent reads:
- `app_name`, `category`, `platform`, `app_size_tier`
- `overall_score`, `category_scores` (7 dimensions)
- `top_issues` (3-5, with severity/impact/evidence)
- `recommendations` (5-10, sorted by priority)
- `experiment_ideas` (3-7, with hypothesis/test_type/metric)
