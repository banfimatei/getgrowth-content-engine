# Email Newsletter Snippet Template

> Generates a 2-3 paragraph block for the weekly GetGrowth newsletter.

---

## Template

```
## This Week's ASO Teardown: {{app_name}}

We put {{app_name}} — a {{app_size_tier}} {{category}} app — through our ASO audit.

**Overall score: {{overall_score}}/100.**

The good: {{strongest_area_summary}}.
The gap: {{weakest_area_summary}}.

Our top recommendation? {{recommendations[0].recommendation}}. This is a {{recommendations[0].effort}}-effort change with {{recommendations[0].expected_impact}} expected impact.

If you're building in {{category}}, the experiment idea we'd steal from this audit:

> "{{experiment_ideas[0].hypothesis}}"

[Read the full teardown →]({{blog_url}})

---

*Every week we publicly audit a real app and share exactly what we'd change. Reply to this email if you want your app featured.*
```

---

## Rules

- 150-250 words max
- Lead with the score as the hook
- One actionable takeaway, one experiment idea
- Always end with the CTA loop (reply = featured)
