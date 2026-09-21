# Saved Research Workflow

Use this workflow when the user wants to save, organize, share, revisit, or visually arrange App Fuel findings.

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

## Canvases

Use canvases when findings should become a visual workspace rather than a list:

- mapping competitors, hooks, offers, personas, or funnel stages
- comparing paid ads and organic reels side by side
- clustering examples by theme or creative strategy
- preserving selected examples with insight notes
- giving the user a workspace link they can continue editing

Read `references/canvases.md` before creating or updating a canvas. Key rules:

- Use `list_canvases` before modifying an existing board.
- Use `get_canvas` before updating so existing nodes, groups, viewport, and user edits are preserved.
- Use stable node and group ids.
- For real App Fuel cards, send `type`/`sourceType` as `ad`, `reel`, or `app` and `sourceId` as public ad id, public reel id, or app id. App Fuel hydrates media and metadata.
- Use `html` nodes for polished static insights, comparison tables, scorecards, report panels, next-step recommendations, and App Fuel media/page embeds.
- Put safe HTML in `metadata.html`. Use up to about `900x600` for rich report panels; the node auto-fits smaller when content is smaller, content scrolls inside the node, and the user can resize it. App Fuel permits inline CSS plus App Fuel media/page iframes, images, videos, and links there; scripts, event handlers, and non-App-Fuel iframe URLs are stripped.
- Use `get_canvas_snapshot` after large layout changes or when visual verification matters.
- Return `canvas.workspaceUrl` for the signed-in user. Return `canvas.url` only when the canvas is public.

## Saved Filters

Use saved filters when a research request is broad, repeatable, or too large to save item by item.

Good saved filter cases:

- "keep this search for active fitness ads around 20k revenue"
- "save this reel market scan"
- "make a reusable research view for these filters"

## Output

Confirm what was saved or arranged, include collection/canvas links when present, and state whether the result is private or public. If a limit blocks saving, say the item was not saved and suggest removing older items or using a saved filter.
