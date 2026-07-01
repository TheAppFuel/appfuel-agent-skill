# Creative Research Workflow

Use this workflow for paid ads, organic Reels, hooks, creative patterns, examples, similar ads, and visual App Fuel galleries.

## Query Rule

`query` searches the whole AI creative/content profile inside ads or reels. It is not a hook-only or field-specific embedding search.

Put only creative-content ideas in `query`, such as:

- scene or visual moment
- claim or offer
- product UI moment
- spoken transcript, caption, OCR, or hook-like wording
- pain point or desired outcome
- creator mechanic or reel format

Put structured constraints in typed arguments or filters:

- category
- active/running status
- app ids
- app product concept
- account type
- media type
- hook type label
- people labels
- video duration
- dates
- app revenue
- grouping
- sorting
- pagination

If the user asks for a category/status list without a creative idea, use `query=""`.

## App-Level Semantic Matching

Use `app_product_query` for the app/product market, not `query`.

Correct for "find active ads with travel features from photo and video editing apps":

```json
{
  "query": "travel features",
  "filters": {
    "app_product_query": "photo and video editing apps",
    "active_status": "active"
  },
  "group_by": "app",
  "limit": 20,
  "return_view": true
}
```

Correct for "Health & Fitness apps running ads":

```json
{
  "query": "",
  "filters": {
    "category": "HEALTH_AND_FITNESS",
    "active_status": "active"
  },
  "group_by": "app",
  "limit": 20,
  "return_view": true
}
```

## Multi-Query

Use a string array only for semantically distinct alternatives:

```json
{
  "query": ["dog training tip", "dog walking routine", "puppy care app demo"],
  "filters": {
    "category": "LIFESTYLE",
    "active_status": "active",
    "media_type": "video"
  },
  "group_by": "app",
  "limit": 20,
  "return_view": true
}
```

Do not expand one phrase into tiny variants such as `POV`, `POV:`, `POV you`, and `POV you're`.

## Paid Ads

- Use `search_ads` for paid creative research.
- Read each result's `overview` before calling detail. It usually contains hook, hook type/source, main claim, value proposition, pain points, target personas, offer, strategy summary, ad description, and structure.
- Use `ad_detail` when the user selects one ad or asks for deeper single-ad analysis.
- Use `similar_ads` when the user asks for more examples like one ad.
- Prefer `public_id` (`ad_...`) as `creative_key` for `ad_detail`, `similar_ads`, save-item, and canvas nodes. Use another returned identifier only when no public id exists.

## Organic Reels

- Use `search_reels` for organic Instagram Reel research.
- Use `account_type`, engagement filters, dates, duration, app revenue, app ids, and category as typed constraints.
- Return Reels with enough context to compare creator mechanics, app UI moments, captions, hooks, and engagement.
- Prefer `public_id` (`reel_...`) for save-item and canvas nodes.

## Pagination

Flat search pages return up to 50 paid creatives or organic reels. Grouped app pages return up to 20 apps, with up to 24 ads or reels per app.

When `pagination.has_more=true`, call the same tool again with `pagination.next_request`. To inspect more than 24 examples from one selected app, make a follow-up flat request with `include_app_ids`, `group_by=""`, and offset pagination.

## Output

Lead with the count and `view_url` when present. Summarize strongest examples or patterns with evidence from result `overview`, `creative`, `ad`, `organic`, and `evidence` fields. Describe people labels as creative-content labels, such as "male-presenting creative label"; do not imply identity recognition.
