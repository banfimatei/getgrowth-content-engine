# LinkedIn Post Templates (Repurposer Agent)

> Each audit generates 2-3 LinkedIn posts. The agent picks the best angles from the audit data.

---

## Template A: Single Insight Hook

```
One thing we keep seeing in {{category}} apps:

{{top_issues[0].issue}}

We audited {{app_name}} ({{overall_score}}/100 ASO score) and found that {{top_issues[0].current_state}}.

Why this matters: {{top_issues[0].impact}}

The fix isn't complicated:
→ {{recommendations[0].recommendation}}

This alone could move the needle on {{recommendations[0].category}}.

Full teardown: {{blog_url}}

#ASO #AppGrowth #{{category_hashtag}} #MobileMarketing
```

## Template B: Scorecard Reveal

```
We just audited a {{app_size_tier}} {{category}} app.

Here's what the ASO scorecard looks like:

📊 Metadata: {{category_scores.metadata}}/100
🎨 Visuals: {{category_scores.visuals}}/100
🔑 Keywords: {{category_scores.keywords}}/100
⭐ Reviews: {{category_scores.ratings_reviews}}/100
🔄 Conversion: {{category_scores.conversion}}/100

Overall: {{overall_score}}/100

The biggest gap? {{lowest_scoring_category}} at just {{lowest_score}}/100.

Our #1 experiment idea:
"{{experiment_ideas[0].hypothesis}}"

→ Full breakdown with 5 more experiment ideas: {{blog_url}}

#ASO #MobileGrowth #AppOptimization
```

## Template C: Experiment Idea

```
Experiment idea for {{category}} apps:

"{{experiment_ideas[0].hypothesis}}"

We'd test this as a {{experiment_ideas[0].test_type}} measuring {{experiment_ideas[0].metric}} over ~{{experiment_ideas[0].estimated_duration_days}} days.

Why we think this works:
{{experiment_context_from_audit}}

Confidence: {{experiment_ideas[0].confidence_level}}

This came from our public ASO teardown of {{app_name}}.

Full analysis → {{blog_url}}

#GrowthExperiments #ASO #AppMarketing
```

---

## Style Rules

- Max 1,300 characters (LinkedIn optimal)
- Start with a hook line that stops scrolling
- Use line breaks liberally
- End with link + 3-5 hashtags
- No emojis in the hook line, use sparingly in body
- Write as the GetGrowth team ("we")
