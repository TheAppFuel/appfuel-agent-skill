# App Fuel Agent Endpoints

Production API base:

```text
https://new.theappfuel.com/api/elite/v1/elite
```

Authentication:

- Public documentation endpoints do not require authentication.
- Search, app detail, usage, and saved research endpoints require an authenticated App Fuel connection.
- Prefer hosted MCP OAuth. Remote MCP clients discover OAuth through `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server`.
- API-key bearer auth remains available as a fallback for REST tests and MCP clients that do not support OAuth.
- If a protected endpoint returns `{"detail":"Agent authentication credentials were not provided."}`, complete the MCP OAuth login or configure a fallback API key and retry.

Public endpoints:

- `GET /agent/instructions.md`: hosted Markdown instructions.
- `GET /agent/instructions.json`: hosted instructions with version/hash/source metadata.
- `GET /agent/openapi.json`: curated OpenAPI spec.
- `GET /agent/schema`: combined agent contract.
- `GET /agent/schema/ads`: paid ads filters, pagination, and response shape.
- `GET /agent/schema/reels`: organic reels filters, pagination, and response shape.
- `GET /agent/schema/apps`: app search/detail fields.
- `GET /agent/schema/app-reviews`: App Store review inputs, limits, and response shape.
- `GET /agent/schema/collections`: saved research operations.
- `GET /agent/schema/canvases`: research canvas operations and state shape.
- `GET /mcp/connection`: hosted MCP connection snippets.
- `GET|POST /mcp`: hosted Streamable HTTP MCP endpoint. Use this as the default agent connection.
- `GET /.well-known/oauth-protected-resource`: MCP protected resource metadata.
- `GET /.well-known/oauth-authorization-server`: OAuth authorization server metadata.
- `POST /oauth/register`: dynamic OAuth client registration for MCP clients.
- `POST /oauth/token`: authorization-code or refresh-token exchange for MCP clients.

Protected endpoints:

- `GET /agent/usage`: monthly request and download usage.
- `POST /agent/apps/search`: app discovery by name, category, audience, job-to-be-done, competitor set, product concept, or app description.
- `POST /agent/apps/detail`: one app's metadata, intelligence, recent revenue series, similar apps, and App Fuel gallery entry points. Set `include_rankings=true` only when ranking data is needed; use `rankings_limit` to keep ranking output compact.
- `POST /agent/apps/reviews`: live public App Store reviews for one app and App Store countries. Use for pain points, objections, praise language, and hook research.
- `POST /agent/ads/search`: enriched paid ad search.
- `POST /agent/ads/detail`: detailed paid ad creative snapshot, media, app context, and sanitized AI analysis.
- `POST /agent/ads/similar`: similar paid ad creatives from one source creative.
- `POST /agent/reels/search`: enriched organic Instagram Reels search.
- `POST /agent/collections/list`: saved collections, items, filters, and pagination.
- `POST /agent/collections/create`: create or reuse a saved collection.
- `POST /agent/collections/update`: make a saved collection public or private.
- `POST /agent/collections/save-item`: save an app, paid ad, or organic reel.
- `POST /agent/canvases/list`: list research canvases.
- `POST /agent/canvases/create`: create a new named research canvas.
- `POST /agent/canvases/get`: read the full state of one research canvas.
- `POST /agent/canvases/update`: update canvas metadata, visibility, viewport, nodes, or groups.
- `POST /agent/canvases/snapshot`: return focused `visualPreviewUrl` links for the full canvas or crop rectangles.

App search request:

```json
{
  "app_product_query": "photo and video editing apps",
  "limit": 30,
  "offset": 0
}
```

App Store reviews request:

```json
{
  "app_id": "897446215",
  "countries": ["us", "gb"],
  "reviews_per_country": 200,
  "ratings": [1, 2, 3],
  "locale": "en-US"
}
```

Review notes:

- `countries` is required and uses two-letter App Store country codes.
- One call scans at most 1,000 total reviews across all countries.
- `ratings` filters after fetching the latest scanned reviews. The backend does not keep crawling until it finds 1,000 matching low-star or high-star reviews.
- Reviews are returned live and are not stored by App Fuel.
- If `reviews_per_country` or `countries * reviews_per_country` exceeds 1,000 scanned reviews, the REST endpoint returns HTTP 400 with `error.code="app_reviews_limit_exceeded"`, plus `requested` and `limit`.

Agent REST and MCP tool errors use `{"error": {...}, "detail": {...}}`. Branch on `error.code`; use `error.invalid_fields[]` to repair request fields before retrying. Common codes are `invalid_request`, `app_reviews_limit_exceeded`, `app_reviews_country_limit_exceeded`, `not_found`, `app_not_found`, `ad_not_found`, `canvas_not_found`, `app_reviews_rate_limited`, `app_reviews_configuration_error`, `app_reviews_unavailable`, and `monthly_request_limit_exceeded`.

MCP tools return the same `error`/`detail` object for validation and not-found failures. The `app_store_reviews` tool also returns this object when review fetching is temporarily unavailable.

Paid ads request:

```json
{
  "query": "",
  "filters": {
    "category": "HEALTH_AND_FITNESS",
    "active_status": "active",
    "min_app_revenue": 15000,
    "max_app_revenue": 30000
  },
  "limit": 20,
  "ads_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Paid ads with app-level semantic constraints:

```json
{
  "query": "travel features",
  "filters": {
    "app_product_query": "photo and video editing apps",
    "active_status": "active",
    "media_type": "video"
  },
  "limit": 20,
  "ads_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Paid ads multi-query request with semantically distinct alternatives:

```json
{
  "query": ["dog training tip", "dog walking routine", "puppy care app demo"],
  "filters": {
    "category": "LIFESTYLE",
    "active_status": "active",
    "media_type": "video"
  },
  "limit": 20,
  "ads_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Paid ad detail request:

```json
{
  "creative_key": "ad_09dad398629428b7d984",
  "include_analysis": true
}
```

Similar ads request:

```json
{
  "creative_key": "ad_09dad398629428b7d984",
  "limit": 12,
  "strategy": "hybrid",
  "exclude_same_app": true
}
```

Organic reels request:

```json
{
  "query": "",
  "filters": {
    "category": "HEALTH_AND_FITNESS",
    "account_type": "app_account",
    "min_views": 10000
  },
  "limit": 20,
  "reels_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Organic reels with app-level semantic constraints:

```json
{
  "query": "travel features",
  "filters": {
    "app_product_query": "photo and video editing apps",
    "account_type": "app_account",
    "min_views": 10000
  },
  "limit": 20,
  "reels_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Organic reels multi-query request with semantically distinct alternatives:

```json
{
  "query": ["dog training tip", "dog walking routine", "puppy care app demo"],
  "filters": {
    "category": "LIFESTYLE",
    "min_views": 10000
  },
  "limit": 20,
  "reels_per_app": 8,
  "group_by": "app",
  "return_view": true
}
```

Save item request:

```json
{
  "item_type": "app",
  "item_key": "1234567890",
  "collection_name": "Fitness apps running ads",
  "notes": "Strong onboarding and active paid ads."
}
```

Create public collection request:

```json
{
  "name": "Fitness apps running ads",
  "is_public": true
}
```

Update collection visibility request:

```json
{
  "collection_id": "collection-id",
  "is_public": false
}
```

Create research canvas request:

```json
{
  "name": "Fitness ad hooks map",
  "description": "Grouped findings from active Health & Fitness ads",
  "is_public": false,
  "viewport": { "x": 260, "y": 160, "zoom": 0.9 },
  "groups": [
    { "id": "group-social-proof", "title": "Social proof hooks", "x": 0, "y": 0, "width": 900, "height": 620 }
  ],
  "nodes": [
    {
      "id": "ad-1",
      "type": "ad",
      "x": 40,
      "y": 80,
      "width": 300,
      "height": 420,
      "title": "Before/after transformation",
      "description": "Strong hook with visible progress proof.",
      "sourceType": "ad",
      "sourceId": "ad_09dad398629428b7d984",
      "groupId": "group-social-proof"
    },
    {
      "id": "note-1",
      "type": "label",
      "x": 390,
      "y": 110,
      "width": 260,
      "height": 130,
      "title": "Pattern",
      "description": "These ads open with immediate proof before product UI."
    },
    {
      "id": "insight-1",
      "type": "html",
      "x": 390,
      "y": 280,
      "width": 340,
      "height": 210,
      "title": "Takeaways",
      "description": "Static insight card.",
      "metadata": {
        "html": "<section><h3>Top pattern</h3><p>Proof first, product UI second.</p></section>"
      }
    }
  ]
}
```

Update research canvas request:

```json
{
  "canvas_id": "canvas-id",
  "is_public": true,
  "nodes": [],
  "groups": [],
  "viewport": { "x": 260, "y": 160, "zoom": 0.9 }
}
```

Query rule:

- Use `query` for whole creative-profile search inside ads or reels: scenes, claims, offers, visible UI, captions, transcripts, pain points, creator mechanics, product moments, or hook-like wording. It is not a hook-only embedding search.
- Send `query` as a string array with short, semantically distinct alternatives when a user request contains several related creative concepts. Do not send one long keyword string, and do not expand one phrase into tiny spelling, punctuation, casing, singular/plural, contraction, or filler-word variants.
- Put category, active/running status, app ids, app-level semantic constraints, account type, media type, hook type, video duration, dates, revenue, engagement, grouping, sorting, and pagination in filters or typed arguments.
- Use `app_product_query` for app product concepts, app descriptions, or jobs-to-be-done, such as "photo and video editing apps". If this already scopes the market, do not add a broad category filter unless the user asked for that category.
- Use `hook_type` only for typed hook-category labels; do not treat it as a hook-only semantic search.
- For "find Health & Fitness apps running ads", use `query=""`, `filters.category="HEALTH_AND_FITNESS"`, `filters.active_status="active"`, and `group_by="app"`.
- Paid ad search results include `overview` for immediate interpretation: hook, claim, value proposition, pain points, target personas, offer, strategy, ad description, and structure.
- For one or more specific apps, use `include_app_ids` with App Fuel app ids from `search_apps` or `app_detail`. For paid ads, use `exclude_app_ids` to remove known apps from a market scan.
- For one specific paid ad, use `ad_detail` with `public_id` (`ad_...`) from search results when available; pass it as `creative_key`. Use `similar_ads` with that same id when the user wants more creatives like it.

Common ads filters include `category`, `active_status`, `include_app_ids`, `app_product_query`, `app_semantic_limit`, `exclude_app_ids`, `media_type`, `people_gender`, `people_role`, `people_age_range`, `creative_format`, `production_quality`, `language`, `cut_speed`, `pain_intensity`, `hook_type`, `min_run_days`, `max_run_days`, `min_video_duration_seconds`, `max_video_duration_seconds`, `web_funnel`, `min_app_revenue`, `max_app_revenue`, `app_release_start`, `app_release_end`, `min_radar_average`, `radar_mins`, and `sort`.

Common reels filters include `category`, `account_type`, `include_app_ids`, `app_product_query`, `app_semantic_limit`, `language`, `hook_type`, `people_gender`, `people_age_range`, `ui_context`, `creator_mode`, `dominant_emotion`, `min_views`, `min_likes`, `min_comments`, `min_video_duration_seconds`, `max_video_duration_seconds`, `min_app_revenue`, `max_app_revenue`, `date_start`, `date_end`, `app_release_start`, `app_release_end`, and `sort`.

Pagination:

- Flat search pages return up to 50 paid creatives or organic reels. Grouped app pages return up to 20 apps, with up to 24 ads or reels per app.
- Use `pagination.next_request` when present. For grouped app results this fetches more apps through `app_offset`. To inspect more than 24 ads or reels from one selected app, make a follow-up flat request with `include_app_ids` for that app, `group_by=""`, and `offset` pagination.
- For grouped app results, the next page normally uses `app_offset`.
- For flat creative results and app search, the next page normally uses `offset`.

Set `return_view=true` when examples matter and return `view_url` to the user.

Collections are private by default. Public collections returned by `create_collection`, `update_collection`, and `list_collections` include `collection.url` when a `shareId` is available. Return that URL as the direct collection link after creating or filling a public collection. Private collections do not include a public URL.

Collection limit: a user can keep up to 20,000 saved items inside collections across all collections. When the limit is reached, moving or saving another item into a collection returns an error.

Canvas notes:

- Canvases are private by default.
- Every canvas response can include `workspaceUrl`, which opens the signed-in owner workspace.
- Public canvases include `canvas.url`, a read-only share link.
- For real ad, reel, and app cards, provide `sourceType` plus `sourceId`; App Fuel hydrates the card media, app icon, and metadata from those ids. Do not copy raw payload metadata on those card nodes. Manual App Fuel media/page embeds belong inside html node `metadata.html`.
- Snapshot responses include `snapshot.viewBox`, `snapshot.width`, `snapshot.height`, and `snapshot.visualPreviewUrl`. Pass `crop: {x, y, width, height}` to inspect one canvas-coordinate area or `crops: [{x, y, width, height}, ...]` to inspect multiple zones in one call; multi-zone responses include `snapshots`, each with its own focused `visualPreviewUrl`.
- Canvas nodes support `label`, `app`, `ad`, `reel`, and `html`.
- For html nodes, put safe markup in `metadata.html`. Html nodes may include App Fuel media/page iframes, images, videos, and links. Do not include scripts or third-party embeds.
- Use `name` in create or update requests when the user asks to name or rename a canvas.
- Use `groups` for themes or clusters.
