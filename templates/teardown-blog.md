# ASO Teardown Blog Post Template

> This template is used by the Teardown Writer Agent. Variables in `{{curly_braces}}` are populated from the audit JSON output.

---

## Title Pattern

`ASO Teardown: {{app_name}} — What a {{app_size_tier}} {{category}} App Gets Right (and Wrong)`

## Structure

### Section 1: Hook (100-150 words)

- Who is this for: founders/PMs building {{category}} apps
- Why this app: {{app_size_tier}} stage, {{overall_score}}/100 overall ASO score
- The one-line verdict: "{{app_name}} nails X but leaves Y on the table"

### Section 2: The Scorecard (visual table)

| Dimension | Score | Verdict |
|-----------|-------|---------|
{{#each category_scores}}
| {{dimension}} | {{score}}/100 | {{one_line_verdict}} |
{{/each}}

**Overall: {{overall_score}}/100** | Visibility: {{visibility_score}} | CVR: {{cvr_score}}

### Section 3: What's Working

- Pull from recommendations where current_state is positive
- 2-3 things the app does well with specific evidence

### Section 4: Top Issues (1 section per issue)

{{#each top_issues}}
#### Issue {{@index}}: {{issue}}

**Severity:** {{severity}} | **Category:** {{category}}

**What we found:** {{current_state}}

**Why it matters:** {{impact}}

**Evidence:** {{evidence}}

**What we'd do:** {{linked_recommendation.details}}
{{/each}}

### Section 5: Experiment Roadmap (3-5 prioritized tests)

{{#each experiment_ideas limit=5}}
{{@index}}. **{{hypothesis}}**
   - Test type: {{test_type}}
   - Track: {{metric}}
   - Duration: ~{{estimated_duration_days}} days
   - Confidence: {{confidence_level}}
{{/each}}

### Section 6: How This Compares (if competitor data exists)

Brief competitive context — where {{app_name}} sits relative to top 3 in {{category}}.

### Section 7: CTA

"Want this analysis for your app? [Get your free ASO audit →](https://getgrowth.eu)"

---

## Tone & Style Rules

- Write like a sharp growth consultant, not a generic blog
- Use "we" (GetGrowth team perspective)
- Be specific: numbers, screenshots references, concrete examples
- No fluff paragraphs — every sentence should teach or persuade
- Target length: 1,200-1,800 words
- Include 2-3 pull quotes suitable for social sharing
