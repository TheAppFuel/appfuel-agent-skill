# App Fuel Research Canvases

Read this reference before creating or updating App Fuel research canvases, especially when arranging many findings, adding groups, using html insight nodes, or inspecting snapshots.

## When To Use A Canvas

Use a canvas when the user wants research to become a visual workspace rather than a flat answer:

- map competitors, hooks, offers, personas, or funnel stages
- compare paid ads and organic reels side by side
- cluster examples by theme or creative strategy
- preserve selected findings for later review
- give the user a workspace link they can open and continue editing

Use a collection instead when the user mainly wants a saved list. Use both when the user wants a saved library plus a visual synthesis.

## Basic Flow

1. Use `describe_canvases_schema` when field names are uncertain.
2. Use `list_canvases` before choosing or modifying an existing canvas.
3. Use `get_canvas` before updating a canvas so existing nodes, groups, viewport, and user edits are preserved.
4. Use `create_canvas` for a new board, or `update_canvas` for layout or visibility changes.
5. Use stable ids such as `ad-hook-proof-1`, `reel-creator-demo-2`, or `group-offers`.
6. Use `get_canvas_snapshot` after large updates or when visual verification matters. Use a small `crop` for one exact area or `crops` for multiple precise zones in one call. Use returned `visualPreviewUrl` links to inspect the canvas visually.
7. Return `canvas.workspaceUrl` for the signed-in user. Return `canvas.url` only for public canvases.

For real App Fuel cards, send the id contract only: `type`/`sourceType` of `ad`, `reel`, or `app`, plus `sourceId` from the search result public id or app id. App Fuel hydrates media, thumbnails, app icons, card metadata, and detail payloads from those ids so cards match the user's manually added cards. Do not copy raw ad/reel/app payloads into node metadata for these cards.

## Canvas State Shape

```json
{
  "name": "Health & Fitness hooks map",
  "description": "Active ads grouped by hook pattern",
  "is_public": false,
  "viewport": { "x": 120, "y": 100, "zoom": 0.85 },
  "groups": [
    { "id": "group-proof", "title": "Proof-first hooks", "x": 0, "y": 0, "width": 980, "height": 620, "color": "#fff0bf" }
  ],
  "nodes": [
    {
      "id": "ad-proof-1",
      "type": "ad",
      "x": 40,
      "y": 90,
      "width": 300,
      "height": 460,
      "title": "Progress proof ad",
      "description": "Opens with visible progress proof before app UI.",
      "sourceType": "ad",
      "sourceId": "ad_09dad398629428b7d984",
      "groupId": "group-proof"
    },
    {
      "id": "note-proof-1",
      "type": "label",
      "x": 380,
      "y": 120,
      "width": 280,
      "height": 130,
      "title": "Pattern",
      "description": "Proof appears before the offer, then product UI explains the mechanism."
    }
  ]
}
```

## Node Types

- `ad`: paid ad card. Use `sourceId` as the `public_id` (`ad_...`) from search results when available; use another returned ad identifier only when no public id is available. Do not provide media URLs or raw payload metadata for real ad cards.
- `reel`: organic reel card. Use `sourceId` as the `public_id` (`reel_...`) from search results when available; use another returned reel identifier only when no public id is available. Do not provide media URLs or raw payload metadata for real reel cards.
- `app`: app card. Use `sourceId` as the App Fuel `app.id`. Do not provide raw app payload metadata for real app cards unless the tool schema explicitly asks for it.
- `label`: editable note with `title` and `description`.
- `html`: insight panel. Put safe markup in `metadata.html`. Html nodes may include App Fuel media/page iframes, images, videos, and links. Scripts, event handlers, object/embed tags, iframe `srcdoc`, and non-App-Fuel URLs are stripped.
Prefer the id contract for real App Fuel results. Do not invent media URLs. Manual App Fuel media URLs belong only inside `metadata.html` for html nodes, not in normal card/media nodes.

## Layout Guidance

Use roomy cards and clear spacing:

- paid ad or reel nodes: about `300x460` or larger
- app nodes: about `320x260` or larger
- label nodes: about `260x120`
- html insight nodes: about `360x220`
- group padding: at least `32` around contained nodes
- space between groups: at least `80`

Good layouts:

- columns by hook type, audience, funnel stage, or app
- rows by paid ads, organic reels, app/product context, and takeaways
- one group per pattern, with 2-6 examples and one short insight node

Avoid overlapping nodes unless the user specifically asks for a pile or moodboard. Preserve user-created positions when updating an existing canvas.

## Groups

Use groups for clusters or themes. Groups are top-level `groups` entries, not nodes. Set `groupId` on nodes that belong inside a group.

## Html Insight Nodes

Use html nodes for compact summaries the user can scan visually: trend cards, scorecards, comparison tables, App Fuel media embeds, or next-step recommendations.

Keep markup static and simple:

```json
{
  "id": "insight-hooks",
  "type": "html",
  "x": 720,
  "y": 100,
  "width": 380,
  "height": 240,
  "title": "Hook pattern",
  "description": "Summary card",
  "metadata": {
    "html": "<section><h3>Top pattern</h3><p>Proof first, app UI second, offer last.</p><ul><li>3 examples use progress proof</li><li>2 examples use creator testimonial</li></ul></section>"
  }
}
```

Do not use scripts or third-party embeds. Iframes are supported for App Fuel pages and media URLs inside `metadata.html`; non-App-Fuel iframe URLs are removed. Keep links minimal and App Fuel related.

## Updating Existing Canvases

When updating an existing canvas:

- call `get_canvas` first
- keep existing nodes unless the user asks to remove them
- reuse stable ids for prior findings
- append new nodes in free space
- update `viewport` only when it improves the opening view
- use `get_canvas_snapshot` if the layout is complex

If the user asks for a public/shareable canvas, set `is_public=true` and return `canvas.url`. Otherwise keep canvases private and return `canvas.workspaceUrl`.

## Visual Inspection

`get_canvas_snapshot` returns `visualPreviewUrl` links that open the signed-in canvas focused on the requested coordinates. Prefer small canvas-coordinate `crop` rectangles around the exact area you need to inspect. Use `crops` for several precise zones in one call. For interactive visual review, return `canvas.workspaceUrl` or the precise `visualPreviewUrl` so the signed-in user can open the canvas in App Fuel.
