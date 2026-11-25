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
- `account`: `all` (default) = the apps' own accounts and creators merged; `app` = the apps' own accounts only (their Instagram reels and their own TikTok accounts); `creator` = creators promoting apps only.
- Creator-video labels (they leave the apps' own Instagram reels out, since those carry no such labels): `content_style` (ugc_talking_head, screen_recording_demo, skit_or_acting, ai_avatar, ...), `production` (lo_fi_phone, polished_creator, studio_produced), `faceless`, `creator_country` (US, GB, ...), `ai_visuals` (none, partial, heavy), `standout`, `creator_handle` (more videos from an account: the result's `creator.handle`), `min_creator_followers`.
- `platform`: `instagram` keeps the apps' own Instagram reels next to Instagram creators; `tiktok` returns TikTok videos only (creators, or with `account=app` the apps' own TikTok accounts).
- `account_type` (app_account, company_account, influencer_creator) labels the apps' own Instagram accounts and limits the search to those reels.
- Engagement and time: `min_views` / `max_views` (a band such as 10k-100k), `min_likes`, `min_comments`, `posted_within_days` (7 = the last week; the server counts the days), or exact `date_start` / `date_end` as `YYYY-MM-DD` (relative phrases are refused). "Latest" = `sort=recent`. Duration: `min_video_duration_seconds` / `max_video_duration_seconds` (0-1800).
- Use engagement filters, dates, duration, app revenue, app ids, and category as typed constraints; put the creative idea in `query`.
- Each result says `source` (`app_reel` = the app's own Instagram reel, `app_video` = the app's own account on UGC Radar, usually its TikTok, `creator_video` = a creator promoting the app), `platform`, and carries `appfuel_url` (opens that video on App Fuel), `organic.permalink` (the original post) and `creative.video_url`. UGC Radar videos add a `creator` block (handle, followers, country, content_style, production, faceless, ai_visuals, standout). Every source carries the same `evidence` block (description, transcript, on-screen text, hook, strategy summary, labels; null only for a creator video not analysed yet), and `creative.video_url` is a signed MP4 link the agent can download and watch.
- Example, "the latest reels about video editing with more than 10k views from the last 7 days": `search_reels` with `query="video editing"`, `min_views=10000`, `posted_within_days=7`, `sort="recent"`, `group_by_app=false`.
- Use `reel_detail` for one video's full transcript, on-screen text and description (search results truncate them), and `similar_reels` for look-alikes across both kinds of account. Both take `public_id` (`reel_...`, or `ugc-<n>` for a creator video).
- For "find me the video that ...", use `group_by_app=false` for one ranked list. For "which creators promote app X", use `account=creator` with `include_app_ids`. `app_detail` returns the app's own TikTok handle (`social.tiktok_username`) and `view_urls.creator_videos`.
- Prefer `public_id` (`reel_...` or `ugc-<n>`) for save-item.
- When a page merges the apps' own reels with UGC Radar videos (`account=all`, or `app` without `account_type`), `pagination.next_request` carries a `cursor`; pass it back unchanged.

## Pagination

Flat search pages return up to 50 paid creatives or organic videos. Grouped app pages return up to 20 apps, with up to 24 ads or videos per app.

When `pagination.has_more=true`, call the same tool again with `pagination.next_request`. To inspect more than 24 examples from one selected app, make a follow-up flat request with `include_app_ids`, `group_by=""`, and offset pagination.

## Output

Lead with the count and `view_url` when present. Summarize strongest examples or patterns with evidence from result `overview`, `creative`, `ad`, `organic`, and `evidence` fields. Describe people labels as creative-content labels, such as "male-presenting creative label"; do not imply identity recognition.
