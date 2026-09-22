# Review Research Workflow

Use this workflow when the user asks for App Store review text, low-star pain points, high-star praise language, objections, missing features, trust gaps, desired outcomes, or review-evidence hooks.

## Tool Choice

- Use `app_store_reviews` for public App Store reviews.
- Use `describe_app_reviews_schema` when unsure about current review inputs or error codes.
- Prefer `app="store:<store_id>"` when an ID is known. App names and App Store URLs also work; inspect the resolved identity.
- For ambiguous names, select a returned candidate and retry its explicit store ID. Never repeat an unchanged ambiguous request.

## Inputs And Limits

- `countries` defaults to `us` and uses two-letter App Store country codes such as `us`, `gb`, or `de`. A URL storefront only helps identity lookup.
- Return 1-500 reviews per country (default 500) across up to 10 countries. Each refresh collects the latest pool of up to 500 unique valid reviews per country.
- Reviews are stored and recent pools are reused for 14 days, including completed empty or low-volume scans. Use `force_refresh=true` to fetch again. Stored matching reviews remain available if fetching fails.
- Use `ratings=1`, `ratings=5`, `ratings=[1,2,3]`, or `ratings="1-3"` for star buckets.
- Rating filters apply within the latest 500-review pool before the return limit, so results may be fewer than requested. Historical reviews do not expand the pool.
- If the tool returns `error.code="app_reviews_limit_exceeded"`, use `reviews_per_country` from 1 through 500. Do not retry the same oversized request.

## Review-To-Hook Workflow

Use reviews as evidence, not generic sentiment filler:

1. Pull recent reviews for selected apps and countries.
2. Use low-star reviews for pains, objections, churn triggers, confusing UX, missing features, cancellation reasons, and trust gaps.
3. Use high-star reviews for desired outcomes, proof language, recommendation language, and moments of delight.
4. Cluster repeated language into concise pain or praise themes.
5. Compare those themes against competitor ads or reels using `search_ads`, `ad_detail`, `similar_ads`, or `search_reels`.
6. Output creative brief candidates with pain cluster, review evidence, recommended hook/body/CTA, format, why it should work, example first 3 seconds, and confidence.

AI drafts; a human approves. Missing reviews, weak review evidence, or missing own performance data should lower confidence rather than break the workflow.

## Output

Include returned review count, countries, ratings filter, and strongest themes. Read `countries[].reviews[]` for `date`, `rating`, and merged `text`; do not describe the sample as all-time reviews. Use short paraphrased evidence snippets instead of long quote dumps. For creative strategy, connect each recommendation to a review theme and an observed ad/reel pattern when available.
