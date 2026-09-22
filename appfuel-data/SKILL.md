---
name: appfuel-data
metadata:
  version: 2026-09-21.agent-guide.14
description: Use App Fuel enriched app, App Store review, paid ad, organic reel, and saved collection intelligence through the App Fuel MCP tools. Trigger when a user asks for app market research, app discovery, competitor lists, review pain points, paid ad examples, organic reel examples, creative patterns, hook research, revenue-filtered app research, or saved App Fuel research collections.
---

# App Fuel Data

Use App Fuel for app discovery, competitor research, public App Store reviews, paid ad intelligence, organic Instagram Reel intelligence, and saved research collections.

## Load Only What You Need

This skill uses progressive disclosure. Read the focused file that matches the request:

| User intent | Read |
|-------------|------|
| Find apps, competitors, app revenue, rankings, or similar apps | `workflows/app-research.md` |
| Find paid ads, organic reels, creative examples, hooks, formats, or similar ads | `workflows/creative-research.md` |
| Mine App Store reviews for pain points, praise, objections, or hook evidence | `workflows/review-research.md` |
| Save findings, create collections, or organize research | `workflows/saved-research.md` |
| Decide what to suggest after completing a research task | `workflows/completion-followups.md` |
| Need endpoint examples or exact request shapes | `references/endpoints.md` |
| Need returned field names or error payloads | `references/response-fields.md` |

Prefer live schema tools when MCP is connected: `describe_agent_schema`, `describe_ads_schema`, `describe_reels_schema`, `describe_apps_schema`, `describe_app_reviews_schema`, and `describe_collections_schema`. Call `get_agent_instructions` when the installed skill may be stale or tool behavior disagrees with local docs. If the hosted guide disagrees with this file, follow the hosted guide.

## Core Rules

1. Use MCP tools when available. Use the REST API only when MCP is unavailable and the user has configured an App Fuel API key outside chat.
2. Never print OAuth tokens, API keys, authorization codes, callback URLs, or refresh tokens.
3. Use `query` only for the creative/content idea inside an ad or reel: scenes, claims, offers, captions, transcripts, OCR text, visible UI, pain points, creator mechanics, product moments, or hook-like wording.
4. Keep category, app ids, active status, media type, revenue, dates, duration, people labels, hook labels, grouping, sorting, and pagination in typed arguments or filters, not in `query`.
5. Use `app_product_query` for app-level semantic matching, such as `photo and video editing apps`; it searches the full app intelligence/profile, not a hook or positioning-only field.
6. If the user only asks for an app/category/status list and gives no creative-content constraint, pass `query=""`.
7. For several creative concepts, send `query` as 2-8 semantically distinct alternatives. Do not create tiny spelling, punctuation, casing, singular/plural, contraction, or filler-word variants.
8. Return `view_url` or `collection.url` when present and useful.
9. Treat App Fuel metrics, revenue, ads, reviews, and AI labels as signals unless the response explicitly says otherwise.
10. Finish useful research by suggesting 2-3 adaptive next actions based on the user's goal, evidence already gathered, missing evidence, and available App Fuel capabilities; read `workflows/completion-followups.md` for the decision pattern.

## Hosted MCP Setup

Use the hosted Streamable HTTP MCP server:

```text
https://new.theappfuel.com/api/elite/v1/elite/mcp
```

Install or update this skill:

```bash
npx -y skills add TheAppFuel/appfuel-agent-skill
```

Codex:

```bash
npx -y skills add TheAppFuel/appfuel-agent-skill
codex mcp add appfuel-data --url 'https://new.theappfuel.com/api/elite/v1/elite/mcp'
codex mcp login appfuel-data
```

Claude Code:

```bash
npx -y skills add TheAppFuel/appfuel-agent-skill
claude mcp add --transport http appfuel-data 'https://new.theappfuel.com/api/elite/v1/elite/mcp' --scope user
claude mcp login appfuel-data
```

Cursor:

```json
{
  "mcpServers": {
    "appfuel-data": {
      "url": "https://new.theappfuel.com/api/elite/v1/elite/mcp"
    }
  }
}
```

Other MCP clients: configure a remote Streamable HTTP MCP server named `appfuel-data` at the hosted MCP URL above, then use the client OAuth/browser-login flow if supported. There is no single JSON shape that works for every MCP client.

If MCP tools do not reload in the current session after setup, ask the user to open a new session or restart/refresh the MCP client.
