# App Fuel Response Fields

The API returns compact agent-facing objects, not raw database rows. Read `/agent/schema`, `/agent/schema/ads`, `/agent/schema/reels`, `/agent/schema/apps`, `/agent/schema/app-reviews`, `/agent/schema/collections`, or `/agent/openapi.json` for the latest contract.

Top-level search response fields:

- `schema_version`
- `generated_at`
- `summary`
- `view_url`: shareable App Fuel gallery URL when `return_view=true`; otherwise `null`.
- `query.queries`: normalized creative-content alternatives when `query` was sent as an array. These should be semantically distinct alternatives, not tiny spelling or punctuation variants.
- `query.normalized_filters`
- `query.search_mode`
- `kpis`
- `results`
- `facets.apps`
- `pagination`: includes page metadata and usually `next_request` when more results are available.

Paid ad result fields:

- `public_id`: opaque public paid-ad id for URLs and save-item, for example `ad_09dad398629428b7d984`.
- `ad_id`: representative paid ad id.
- `creative_key`: returned paid-ad identifier for detail and similar-ad follow-up when public_id is not available.
- `app.id`, `app.name`, `app.category`, `app.icon_url`, `app.latest_revenue`
- `ad.is_active`, `ad.media_type`, `ad.first_seen_at`, `ad.last_seen_at`, `ad.flight_days`, `ad.variants_count`
- `creative.title`, `creative.body_text`, `creative.description`, `creative.hook_text`, `creative.hook_type`, `creative.cta_text`, `creative.thumbnail_url`, `creative.media_url`
- `overview.hook_text`, `overview.hook_type`, `overview.hook_source`, `overview.main_claim`, `overview.value_proposition`, `overview.pain_points`, `overview.target_personas`, `overview.cta_phrases`, `overview.offer`, `overview.strategy_summary`, `overview.angles`, `overview.ad_description`, `overview.ad_structure`
- `evidence.people.roles`, `evidence.people.genders`, `evidence.people.age_ranges`, `evidence.people.main_people`, `evidence.people.label_note`
- `evidence.creative_ai.hook_text`, `evidence.creative_ai.hook_type`, `evidence.creative_ai.hook_source`, `evidence.creative_ai.written_hook`, `evidence.creative_ai.spoken_hook`, `evidence.creative_ai.creative_formats`, `evidence.creative_ai.production_quality`, `evidence.creative_ai.cut_speed`, `evidence.creative_ai.pain_intensity`, `evidence.creative_ai.radar_average`, `evidence.creative_ai.ad_structure`, `evidence.creative_ai.scene_count`
- `evidence.text.main_claim`, `evidence.text.value_proposition`, `evidence.text.pain_points`, `evidence.text.target_personas`, `evidence.text.cta_phrases`, `evidence.text.proof_or_credibility_signals`, `evidence.text.strategy_summary`, `evidence.text.angles`, `evidence.text.awareness_stage`, `evidence.text.offer_or_incentive`, `evidence.text.offer`, `evidence.text.pain_point`, `evidence.text.visual_description`, `evidence.text.spoken_transcript`, `evidence.text.ocr_text`

Paid ad detail fields:

- `public_id`, `creative_key`, `ad_id`
- `summary`: same compact shape as a paid ad search result.
- `detail.app`: app context attached to the creative.
- `detail.ad`: status, timing, duration, variant, usage, platform, and destination context.
- `detail.creative`: title, body text, caption, CTA, and media items.
- `detail.analysis`: sanitized AI analysis when `include_analysis=true`.
- `view_url`: App Fuel gallery entry point for the app when available.

Similar paid ad fields:

- `source_creative_key`
- `similar_ads`: cross-app or best matching creatives with the paid ad result shape plus `similarity`.
- `own_app_variants`: same-app variants when available.
- `method`: similarity strategy metadata.
- `query`: normalized similar-ad request settings.

Organic reel result fields:

- `public_id`: opaque public organic reel id for URLs and save-item, for example `reel_8080198d0b44ab81e6c5`.
- `reel_id`: returned organic reel identifier for follow-up when public_id is not available.
- `app.id`, `app.name`, `app.category`, `app.icon_url`, `app.latest_revenue`
- `organic.instagram_username`, `organic.account_type`, `organic.permalink`, `organic.posted_at`, `organic.views`, `organic.likes`, `organic.comments`
- `creative.title`, `creative.caption_text`, `creative.music_title`, `creative.music_artist`, `creative.video_duration`, `creative.thumbnail_url`, `creative.video_url`
- `evidence.people.genders`, `evidence.people.age_ranges`, `evidence.people.label_note`
- `evidence.creative_ai.hook_text`, `evidence.creative_ai.hook_type`, `evidence.creative_ai.hook_source`, `evidence.creative_ai.creator_mode`, `evidence.creative_ai.ui_context`, `evidence.creative_ai.ui_clarity`, `evidence.creative_ai.cut_speed`, `evidence.creative_ai.ending_type`, `evidence.creative_ai.mechanics`
- `evidence.text.organic_strategy_summary`, `evidence.text.reel_description`, `evidence.text.caption_summary`, `evidence.text.spoken_transcript`, `evidence.text.ocr_text`, `evidence.text.search_tags`

App search and app detail fields:

- `app.id`, `app.name`, `app.subtitle`, `app.seller`, `app.categories`
- `app.icon_url`, `app.rating_average`, `app.rating_count`, `app.release_date`
- `app.latest_revenue`, `app.latest_revenue_month`
- `app.match`: app semantic match metadata when app_product_query was used, including product query, score, rank, and combined score.
- `app.store_links.ios`, `app.store_links.android`
- `app.social.instagram_username`, `app.social.has_instagram_reels`
- `app.intelligence.oneLiner`
- `app.intelligence.primaryUseCases`
- `app.intelligence.targetAudiences`
- `app.intelligence.nicheStatus`
- `app.intelligence.jobToBeDone`
- `app.intelligence.opportunityLens`
- `app.intelligence.whyInteresting`
- `app.intelligence.whyUnique`
- `app.intelligence.buildability`
- `app.intelligence.networkEffects`
- `app.intelligence.noveltyScore`, `app.intelligence.commodityScore`, `app.intelligence.incumbentScore`
- `revenue_series`: recent monthly app revenue rows when requested.
- `latest_rankings`: latest category/country/collection ranking snapshot rows when requested. This is capped by `rankings_limit` and is not daily ranking history.
- `similar_apps`: compact app objects.
- `view_urls.ads`, `view_urls.organic_reels`: App Fuel gallery entry points for that app.

App Store review fields:

- `status`, `schema_version`, `generated_at`
- `app.store_id`, `app.name`, `app.developer_name`, `app.app_store_url`, `app.icon_url`, `app.is_in_appfuel_library`
- `request.countries`, `request.reviews_per_country`, `request.ratings`, `request.force_refresh`, `request.resolution`
- `metrics.returned_reviews`, `metrics.rating_distribution`
- `countries[].country`, `countries[].returned_reviews`, `countries[].reviews[]`
- Each review contains `date` (day or null), `rating` (1-5), and `text` (title and body merged without duplication).

Use low-star review text for pain clusters and high-star text for desired outcomes and praise. Rating filters apply within the latest 500-review pool before the return limit, so they may return fewer rows than requested. Reviews are stored and recent pools are reused for 14 days; request `force_refresh=true` to refresh. Version `2026-09-21.app-store-reviews.3` replaces duplicated raw top-level reviews with these compact per-country results.

Agent REST and MCP error fields:

- Errors return both `error` and `detail`; they contain the same object for compatibility.
- `error.code`: machine-readable code such as `invalid_request`, `app_reviews_limit_exceeded`, `app_reviews_country_limit_exceeded`, `not_found`, `app_not_found`, `ad_not_found`, `app_reviews_rate_limited`, `app_reviews_configuration_error`, `app_reviews_unavailable`, or `monthly_request_limit_exceeded`.
- `error.message`: human-readable explanation.
- `error.status`: HTTP status code.
- `error.invalid_fields[]`: repair hints for validation errors. Items can include `field`, alternative `fields`, `reason`, `hint`, and rejected `value`.
- Limit errors can include `requested` and `limit`; monthly usage errors include `used`, `limit`, and `resetAt`.

Saved research fields:

- `collections`: collection id, name, `isPublic`, item count, created/updated timestamps, and `url` for public collections.
- `items`: saved app/ad/reel records with title, subtitle, app id, tags, notes, thumbnail/media URLs, metadata, source payload, collection id, and timestamps.
- `filters`: saved filter views with surface, name, description, filters, summary, and timestamps.
- `kpis`: saved item/filter counts, collected-item count, and collection item limit.
- `pagination`: item pagination and `next_request` when more saved items are available.

Missing values can be `null`, empty arrays, or absent. People labels are creative-content labels, not identity recognition.

Do not promise unsupported raw database fields. If the user needs data not listed here, say the current agent surface does not return it and use schema tools plus `view_url` for inspection.
