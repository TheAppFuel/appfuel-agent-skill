# Review Research Workflow

Use this workflow when the user asks for App Store review text, low-star pain points, high-star praise language, objections, missing features, trust gaps, desired outcomes, or review-evidence hooks.

## Tool Choice

- Use `app_store_reviews` for public App Store reviews.
- Use `describe_app_reviews_schema` when unsure about current review inputs or error codes.
- Use `app_detail` first when the user gives an app name rather than an app id or App Store URL.

## Inputs And Limits

- `countries` is required and uses two-letter App Store country codes such as `us`, `gb`, or `de`.
- One call scans at most 1,000 total reviews across all countries.
- Use `ratings=1`, `ratings=5`, `ratings=[1,2,3]`, or `ratings="1-3"` for star buckets.
- Rating filters apply after scanning the latest reviews, so filtered rows may be fewer than requested.
- If the tool returns `error.code="app_reviews_limit_exceeded"`, reduce `reviews_per_country` or split countries into separate calls. Do not retry the same oversized request.

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

Include review count, countries, ratings filter, scan size, and strongest themes. Use short paraphrased evidence snippets instead of long quote dumps. For creative strategy, connect each recommendation to a review theme and an observed ad/reel pattern when available.
