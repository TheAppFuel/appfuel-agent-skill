# Completion Follow-Ups

Use this workflow before ending a useful App Fuel research answer. Follow-ups must be adaptive, not a fixed checklist.

## Rule

Suggest 2-3 next actions only when they fit the user's goal, the evidence already gathered, and App Fuel's available tools. Each suggestion should feel like the obvious next research move from the current result, not a generic menu.

Do not always suggest the same things. Do not suggest saving, reviews, or similar ads just because those tools exist. Choose based on what would materially improve the user's next decision.

When the user says yes, act immediately with the relevant MCP tools.

## Decide From Context

Before suggesting next actions, infer:

- **User goal:** discovery, competitor research, creative inspiration, ad brief generation, review mining, saving/organizing, or research synthesis.
- **Current artifact:** app list, selected app, paid ads, organic reels, reviews, a collection, or empty/weak results.
- **Missing evidence:** app context, creative examples, review language, similar examples, organic counterpart, paid counterpart, rankings/revenue, or research organization.
- **User momentum:** are they exploring broadly, choosing winners, preparing briefs, or asking for an artifact they can share?
- **Available identifiers:** app ids, ad `public_id`, reel `public_id`, `view_url`, collection id.

Then suggest the smallest high-leverage next step.

## Capability Map

Use these capabilities as ingredients, not as a static list:

- **Save findings:** `save_item`, `create_collection`, `update_collection`, `save_filter`.
- **Deepen creative research:** `ad_detail`, `similar_ads`, `search_ads`, `search_reels`.
- **Review-to-creative chain:** `app_store_reviews` -> pain/praise clusters -> ad hook/body/CTA brief ideas.
- **App context:** `app_detail`, `search_apps`, similar apps, recent revenue, rankings when requested.
- **Pagination/narrowing:** `pagination.next_request`, stricter filters, broader filters, selected app drilldown.

## Adaptive Patterns

Use patterns like these, rewritten for the actual result:

### App List Found

If apps were discovered but no creative evidence was inspected:

- "Should I inspect paid ads and organic reels for the top 3 apps so we can see their acquisition angles?"
- "Should I pull low-star and high-star reviews for the strongest app to turn user language into hooks?"
- "Should I save this market scan as a collection or reusable filter?"

### One App Selected

If the user is evaluating one app:

- "Should I check reviews for this app and turn what people praise or dislike into ad hook/body/CTA ideas?"
- "Should I pull this app's paid ads and organic reels side by side to compare what they test in each channel?"
- "Should I find similar apps so we can see whether this positioning is common or differentiated?"

### Paid Ads Found

If ads were found but not deeply inspected:

- "Should I open the strongest ads with `ad_detail` and extract hook, offer, proof, CTA, and format?"
- "Should I pull similar ads for the best examples to see whether the pattern repeats across apps?"

If ads are strong enough to preserve:

- "Should I save the winners into a collection so we can revisit or share them later?"

If ads need strategic grounding:

- "Should I compare these ads against App Store review pain points before drafting new ad concepts?"

### Organic Reels Found

If reels were found:

- "Should I compare these organic reels against paid ads for the same apps to see what scales from organic to paid?"
- "Should I save the strongest reels and make a short brief from their opening 3 seconds?"

### Reviews Found

If low-star reviews were pulled:

- "Should I turn these complaint clusters into ad hooks, body copy, and CTAs?"
- "Should I compare these pain points against competitor ads to find underused angles?"

If only complaints were pulled:

- "Should I pull 4-5 star reviews too, so the briefs include both pain and proof language?"

If praise was pulled:

- "Should I convert the praise language into proof-first ad concepts?"

### Collection Exists

If a collection exists:

- "Should I add these findings to that collection or make a separate collection for this angle?"

### Empty Or Weak Results

If results are empty, weak, or over-filtered:

- "Should I broaden the filters by removing revenue/status/category constraints?"
- "Should I search by app product concept instead of creative wording?"
- "Should I try paid ads and organic reels separately?"

Do not suggest saving when there is not enough evidence yet.

## Response Pattern

Use one concise line or short paragraph. Tie each action to why it matters:

```text
Smart next moves: 1. pull reviews for the top app to turn praise/complaints into ad angles, 2. save the best ad examples to a collection, 3. find similar ads for the strongest hook.
```

For final answers, use natural language. Avoid repeating the same suggestions across different tasks.
