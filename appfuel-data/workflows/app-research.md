# App Research Workflow

Use this workflow for app discovery, competitor discovery, revenue-banded markets, app detail, rankings, and similar apps.

## Tool Choice

- Use `search_apps` for app discovery by name, category, audience, job-to-be-done, competitor set, product concept, or app description.
- Use `app_product_query` when the user describes what kind of app they want, such as `photo and video editing apps` or `fasting tracker for women`.
- Use `app_detail` after an app is selected, or when the user asks for revenue, app intelligence, similar apps, or gallery entry points.
- Set `include_rankings=true` only when the user asks for rankings. Keep `rankings_limit` modest unless the user asks for a larger ranking sample.
- Use `describe_apps_schema` when unsure about current fields, filters, or limits.

## Revenue And Market Filters

For requests such as "apps doing 20k a month and running ads":

- Use `min_app_revenue` and `max_app_revenue` where the specific surface supports them.
- For "around 20k", start with `min_app_revenue=15000` and `max_app_revenue=30000`.
- For "20k+", use `min_app_revenue=20000`.
- For smaller-app research, add an upper bound so results do not drift toward category leaders.

Do not inspect local config files or credentials to discover hidden API options. Use schema tools and documented filters.

## App-To-Creative Drilldown

When app search identifies interesting apps:

1. Use `app_detail` on selected apps for context, revenue, similar apps, and gallery URLs.
2. Use `search_ads` with `include_app_ids` to inspect paid ads for selected apps.
3. Use `search_reels` with `include_app_ids` to inspect organic content for selected apps.
4. Use `app_store_reviews` when user language, pain points, objections, or praise evidence is needed.

Keep app ids out of creative `query`; use `include_app_ids`, `exclude_app_ids`, or `app_product_query`.

## Output

Lead with the scope and result count. Include the filters used, especially country/category/revenue/status constraints. Prefer compact tables for app comparisons and short evidence-backed takeaways.

Useful next actions often include:

- inspect ads or reels for the top apps
- pull reviews for selected apps
- save selected apps to a collection
- build a canvas mapping apps, creative examples, and opportunity notes
