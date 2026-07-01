# App Fuel Agent Skill

Public agent instructions for using App Fuel app, App Store review, paid ad, organic reel, and saved research intelligence from Codex, Claude Code, Cursor, Windsurf, and other agent clients.

Recommended repository name:

```text
appfuel-agent-skill
```

## What This Gives Agents

- How to search App Fuel apps, paid ads, and organic Instagram Reels.
- How to fetch public App Store reviews for pain-point, objection, praise-language, and hook research.
- When to use `query` versus structured filters.
- How to return App Fuel gallery links for human review.
- How to paginate and save useful findings to App Fuel collections.
- How to create visual research canvases with hydrated App Fuel app/ad/reel cards.
- How to suggest useful next actions when a research task is complete.
- How to avoid exposing internal/admin App Fuel surfaces.

## Repository Structure

```text
appfuel-agent-skill/
|-- README.md
`-- appfuel-data/
    |-- SKILL.md
    |-- README.md
    |-- agents/
    |   `-- openai.yaml
    |-- workflows/
    |   |-- app-research.md
    |   |-- creative-research.md
    |   |-- review-research.md
    |   |-- saved-research.md
    |   `-- completion-followups.md
    `-- references/
        |-- canvases.md
        |-- endpoints.md
        `-- response-fields.md
```

`SKILL.md` is the entrypoint. The workflow and reference files are bundled resources that agents can read on demand.

## How Skill Loading Works

Skill-aware agents do not usually load every file in a skill folder into context up front.

1. The client indexes the skill metadata in `SKILL.md`, especially `name` and `description`.
2. When a user request matches the description, the agent reads `SKILL.md`.
3. `SKILL.md` routes the agent to supporting files such as `workflows/creative-research.md` or `references/canvases.md`.
4. The agent can read files inside the installed skill folder by relative path, as long as the folder was installed/copied with those files.

This means splitting a skill into folders and Markdown files works well. The important rule is that the top-level `SKILL.md` must clearly mention the supporting files and when to read them.

If `appfuel-data/` is installed as one skill, the files under `workflows/` are supporting docs, not separately discoverable skills. If you want separately discoverable skills, create separate folders and give each one its own `SKILL.md`.

## Hosted MCP

Use the hosted Streamable HTTP MCP server:

```text
https://new.theappfuel.com/api/elite/v1/elite/mcp
```

Install or update the skill:

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

When the client asks for authorization, approve App Fuel in the browser. API keys remain available in the App Fuel `/mcp` page for REST tests and fallback clients that do not support remote MCP OAuth.

## Agent Skill

Install the skill with:

```bash
npx -y skills add TheAppFuel/appfuel-agent-skill
```

Skill repo:

```text
https://github.com/TheAppFuel/appfuel-agent-skill
```

Skill source folder:

```text
https://github.com/TheAppFuel/appfuel-agent-skill/tree/main/appfuel-data
```

For older agents without the skills CLI, ask the agent:

```text
Install appfuel-data from https://github.com/TheAppFuel/appfuel-agent-skill/tree/main/appfuel-data
```

Manual install:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R appfuel-data "${CODEX_HOME:-$HOME/.codex}/skills/appfuel-data"
```

## Agent Handoff

Paste this into an agent after the MCP connector is configured:

```text
Use App Fuel as an agent data source for app, App Store review, paid ad, organic reel, and saved research intelligence.

API base: https://new.theappfuel.com/api/elite/v1/elite
Agent instructions: https://new.theappfuel.com/api/elite/v1/elite/agent/instructions.md
OpenAPI spec: https://new.theappfuel.com/api/elite/v1/elite/agent/openapi.json
Agent schema: https://new.theappfuel.com/api/elite/v1/elite/agent/schema
Hosted MCP URL: https://new.theappfuel.com/api/elite/v1/elite/mcp
OAuth metadata: https://new.theappfuel.com/api/elite/v1/elite/.well-known/oauth-authorization-server
Skill install: npx -y skills add TheAppFuel/appfuel-agent-skill
Skill repo: https://github.com/TheAppFuel/appfuel-agent-skill
Skill source folder: https://github.com/TheAppFuel/appfuel-agent-skill/tree/main/appfuel-data

Before answering App Fuel questions:
1. If the App Fuel skill is missing or stale, install/update it with: npx -y skills add TheAppFuel/appfuel-agent-skill
2. Read the agent instructions URL and use the OpenAPI/schema URLs to discover current endpoints and filters.
3. If the MCP client asks for authorization, open the browser login and approve App Fuel.
4. If App Fuel MCP tools are available, use them instead of raw HTTP.
5. If the current session cannot see the hosted MCP server after configuration, say that a new agent session or MCP client restart/refresh is needed.
6. Use query for whole creative-profile search inside ads or reels, not hook-only or field-specific search. Put category, running status, dates, grouping, sorting, pagination, and app-level semantic constraints in filters or typed arguments.
7. Use query as a string array for several short creative-content alternatives instead of one long keyword string.
8. Use app_product_query for full app-profile embedding search such as "photo and video editing apps"; it is not a hook/positioning-only embedding. Do not add a broad category filter unless the user asked for it.
9. Flat search pages are capped at 50 paid creatives or organic reels. Grouped app pages are capped at 20 apps, with up to 24 ads/reels per app. Use pagination.next_request when a response has more results; for deeper examples from one app, make a follow-up flat request with include_app_ids for that app and offset pagination.
10. Return view_url links when present so the user can inspect the matching creative gallery.
11. Use app_store_reviews when the user needs public App Store review text, low-star pain points, high-star praise language, objections, trust gaps, desired outcomes, or hook inspiration. Countries are required App Store country codes like us, gb, or de; one call scans at most 1,000 total reviews and counts against API usage.
12. For creative briefs, cluster review pains, connect them to winning ad/reel angles, and return hook/body/CTA ideas with review evidence and confidence. AI drafts; a human approves.
13. Use collection tools when the user asks to save or organize research.
14. Use canvas tools when the user wants a visual board, grouped findings, mapped patterns, or a workspace link.
15. When a useful research task is complete, suggest 2-3 next actions based on App Fuel capabilities, such as saving to a collection, creating a canvas, pulling similar ads, comparing against reviews, narrowing filters, or fetching the next page.
16. Do not ask the user to paste an App Fuel API key into chat. If OAuth is unavailable, ask the user to use the API-key fallback from the App Fuel MCP page.
17. Do not print OAuth tokens, API keys, authorization codes, callback URLs, or refresh tokens in status messages, command transcripts, or final notes.
```

## Notes

The hosted instruction document is the source of truth for non-Codex agents that can fetch Markdown but do not support Codex skills.

The skill keeps detailed endpoint and field references in `appfuel-data/references/` so agents can load that detail only when they need it.
