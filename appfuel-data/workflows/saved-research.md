# Saved Research Workflow

Use this workflow when the user wants to save, organize, share, or revisit App Fuel findings.

## Collections

Use collections for saved lists of apps, paid ads, and organic reels.

- Use `list_collections` before saving when the user refers to an existing collection.
- Use `create_collection` when the user names a new collection.
- Use `update_collection` only when the user asks to publish, share, unshare, or make a collection private.
- Use `save_item` for apps, ads, and reels.
- Use app `id` for apps.
- Use paid ad `public_id` (`ad_...`) as `item_key` when present.
- Use organic reel `public_id` (`reel_...`) as `item_key` when present.
- Let App Fuel hydrate saved item title, media, links, metadata, and source payload from `item_type` plus `item_key`; do not provide override fields for normal saved items.

Collections are private by default. Set `is_public=true` only when the user asks for a public/shareable collection or direct link. Return `collection.url` when a public URL is present.

## Saved Filters

Use saved filters when a research request is broad, repeatable, or too large to save item by item.

Good saved filter cases:

- "keep this search for active fitness ads around 20k revenue"
- "save this reel market scan"
- "make a reusable research view for these filters"

## Output

Confirm what was saved, include collection links when present, and state whether the result is private or public. If a limit blocks saving, say the item was not saved and suggest removing older items or using a saved filter.
