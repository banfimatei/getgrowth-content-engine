# X Thread Template (Repurposer Agent)

> Generates a 5-7 tweet thread from each audit.

---

## Thread Structure

### Tweet 1 (Hook)
```
We just ran an ASO audit on {{app_name}} ({{category}}).

Score: {{overall_score}}/100

Here's what we found — and what any {{app_size_tier}} {{category}} app can learn from it.

🧵👇
```

### Tweet 2 (Biggest Win)
```
What {{app_name}} gets RIGHT:

{{strongest_area_description}}

Score: {{highest_score}}/100 in {{highest_scoring_category}}

This is what good looks like.
```

### Tweet 3 (Biggest Problem)
```
But here's the #1 issue:

{{top_issues[0].issue}}

{{top_issues[0].impact}}

This is costing them downloads.
```

### Tweet 4 (The Fix)
```
The fix:

{{recommendations[0].recommendation}}

Effort: {{recommendations[0].effort}}
Expected impact: {{recommendations[0].expected_impact}}

{{recommendations[0].details}}
```

### Tweet 5 (Experiment)
```
If we were running growth for this app, we'd test:

"{{experiment_ideas[0].hypothesis}}"

→ {{experiment_ideas[0].test_type}} tracking {{experiment_ideas[0].metric}}
→ ~{{experiment_ideas[0].estimated_duration_days}} days to significance
```

### Tweet 6 (Competitive Context)
```
How does this compare?

In {{category}}, the avg ASO score is ~{{category_avg_estimate}}.

{{app_name}} at {{overall_score}} is {{above_or_below}} average.

Biggest gap vs top competitor: {{competitive_gap}}
```

### Tweet 7 (CTA)
```
Full teardown with all scores + 5 more experiment ideas:

{{blog_url}}

Want this for your app? We're running free public audits every week.

→ getgrowth.eu
```

---

## Rules

- Each tweet max 280 chars
- Thread hook must be curiosity-driven
- Include 1 specific data point per tweet
- No hashtag spam (max 1-2 in final tweet)
