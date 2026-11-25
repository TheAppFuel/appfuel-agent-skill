# Creative Research Workflow

Use this workflow for paid ads, organic Reels, creator (UGC) videos from TikTok and Instagram, hooks, creative patterns, examples, similar ads or videos, and visual App Fuel galleries.

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
- Prefer `public_id` (`ad_...`) as `creative_key` for `ad_detail`, `similar_ads`, and save-item. Use another returned identifier only when no public id exists.

## Organic Videos (Reels And Creator Videos)

- Use `search_reels` for organic research. `account` picks whose videos: `all` (default) merges the apps' own Instagram Reels with creators' TikTok/Instagram videos that promote an app; `creator` returns creator videos only; `app` the apps' own reels only.
- Creator-only filters (they switch `account` to `creator`): `platform` (tiktok, instagram), `content_style` (ugc_talking_head, screen_recording_demo, skit_or_acting, ai_avatar, ...), `production` (lo_fi_phone, polished_creator, studio_produced), `faceless`, `creator_country` (US, GB, ...), `ai_visuals` (none, partial, heavy), `standout`.
- `account_type` (app_account, company_account, influencer_creator) labels the apps' own Instagram accounts and only applies with `account=app`.
- Use engagement filters, dates, duration, app revenue, app ids, and category as typed constraints; put the creative idea in `query`.
- Each result says `source` (`app_reel` or `creator_video`), `platform`, and carries `appfuel_url` (opens that video on App Fuel), `organic.permalink` (the original post) and `creative.video_url`. Creator videos add a `creator` block (handle, followers, country, content_style, production, faceless, ai_visuals, standout); they do not expose a transcript yet.
- Use `reel_detail` for one video's full transcript, on-screen text and description (search results truncate them), and `similar_reels` for look-alikes across both kinds of account. Both take `public_id` (`reel_...`, or `ugc-<n>` for a creator video).
- For "find me the video that ...", use `group_by_app=false` for one ranked list. For "which creators promote app X", use `account=creator` with `include_app_ids`. `app_detail` returns the app's own TikTok handle (`social.tiktok_username`) and `view_urls.creator_videos`.
- Prefer `public_id` (`reel_...` or `ugc-<n>`) for save-item.
- With `account=all`, `pagination.next_request` carries a `cursor`; pass it back unchanged.

## Pagination

Flat search pages return up to 50 paid creatives or organic videos. Grouped app pages return up to 20 apps, with up to 24 ads or videos per app.

When `pagination.has_more=true`, call the same tool again with `pagination.next_request`. To inspect more than 24 examples from one selected app, make a follow-up flat request with `include_app_ids`, `group_by=""`, and offset pagination.

## Output

Lead with the count and `view_url` when present. Summarize strongest examples or patterns with evidence from result `overview`, `creative`, `ad`, `organic`, and `evidence` fields. Describe people labels as creative-content labels, such as "male-presenting creative label"; do not imply identity recognition.
