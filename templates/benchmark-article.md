# Benchmark Article Template (Aggregator Agent)

> Generated from clusters of 5-15 audits in the same category/region.

---

## Title Pattern

`How {{category}} Apps in {{region}} Score on ASO in 2026 — A {{n_apps}}-App Benchmark`

## Structure

### Section 1: What We Measured

- How many apps: {{n_apps}} in {{category}} ({{platform}})
- Region: {{region}}
- Period: audits from {{date_range}}
- Methodology: GetGrowth audit engine (link to methodology page)

### Section 2: The Scorecard Summary Table

| App | Overall | Metadata | Visuals | Keywords | Reviews | Conversion |
|-----|---------|----------|---------|----------|---------|------------|
{{#each audits sorted_by_score}}
| {{app_name}} | {{overall_score}} | {{metadata}} | {{visuals}} | {{keywords}} | {{ratings_reviews}} | {{conversion}} |
{{/each}}
| **Category Avg** | **{{avg_overall}}** | **{{avg_metadata}}** | **{{avg_visuals}}** | **{{avg_keywords}}** | **{{avg_reviews}}** | **{{avg_conversion}}** |

### Section 3: Key Findings (3-5 insights)

{{#each key_findings}}
#### {{@index}}. {{finding_title}}

{{finding_body}}

*Apps this affects: {{affected_apps}}*
{{/each}}

### Section 4: Top Recurring Experiment Ideas

"Here are the experiments we keep recommending to {{app_size_tier}} {{category}} apps:"

{{#each clustered_experiments}}
{{@index}}. **{{experiment_title}}**
   - Seen in {{frequency}}/{{n_apps}} audits
   - Average expected impact: {{avg_impact}}
   - {{one_line_description}}
{{/each}}

### Section 5: What the Best Apps Do Differently

Compare top-3 vs bottom-3 on each dimension. Pull specific patterns.

### Section 6: CTA

"We run these benchmarks quarterly. Get your app included in the next one — [free audit at getgrowth.eu](https://getgrowth.eu)"

---

## Rules

- Data-heavy, not opinion-heavy
- Every claim backed by a number from the audits
- Include at least one chart/visualization suggestion per article
- 2,000-3,000 words
- Link back to individual teardowns where they exist
