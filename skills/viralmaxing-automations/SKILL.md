---
name: viralmaxing-automations
description: "Read the Instagram comment / Direct funnels: which rules fire, how the deliveries convert, and who the leads are. Use when the user says things like: \"how are my Instagram automations doing\", \"show me the leads from that funnel\", \"which code word converts best\", \"did that DM get delivered\" или по-русски: «как работают мои автоматизации в инстаграме», «покажи лиды из воронки», «какое кодовое слово лучше конвертит», «дошло ли сообщение». Reach for this skill whenever Instagram comment or Direct automations, code words, or the leads they bring comes up, even if the user never says \"Viralmaxing\". Requires a Viralmaxing account and the Viralmaxing MCP server connected (https://api.viralmaxing.com/api/mcp) — without it none of these tools are callable."
---

# Instagram automation funnels

Read the Instagram comment / Direct funnels: which rules fire, how the deliveries convert, and who the leads are.

## When to use this skill

- "how are my Instagram automations doing"
- "show me the leads from that funnel"
- "which code word converts best"
- "did that DM get delivered"

По-русски: «как работают мои автоматизации в инстаграме», «покажи лиды из воронки», «какое кодовое слово лучше конвертит», «дошло ли сообщение».

## Connect the server first

This skill is instructions, not tools. Installing it does not add the connector: the tools below
only exist once the Viralmaxing MCP server is connected. If a tool named here is missing, this is
why — say so instead of improvising with a different one.

```bash
claude mcp add --transport http viralmaxing https://api.viralmaxing.com/api/mcp
```

Cursor, Codex and other hosts take the same URL in their own MCP settings. Endpoint:

```
POST https://api.viralmaxing.com/api/mcp
```

- **OAuth 2.1** — dynamic client registration + PKCE, discovery at
  `https://api.viralmaxing.com/.well-known/oauth-protected-resource`. This is what Claude, Cursor and Codex
  use when the user adds the connector by URL.
- **API key** — send `X-API-Key: vmx_...`, created at https://viralmaxing.com/settings/api. The same key works
  for the REST API at `https://api.viralmaxing.com/api` (OpenAPI: https://docs.viralmaxing.com/openapi.yaml).

Every tool is scoped to the authenticated user's workspace. There is no public data surface: if
the workspace does not track an account, no tool here will return metrics for it.

## Tools

| Tool | What it does | Category |
|---|---|---|
| `list_automations` | Automations | read |
| `get_automation_stats` | Automation results | read |
| `list_automation_leads` | Automation leads | read |
| `get_automation_lead_thread` | Lead conversation | read |
| `save_automation_lead` | Record a lead outcome | write |

Available from every Viralmaxing skill: `get_energy_balance`, `get_operation`, `search`, `fetch`.

## Cost

Every tool in this skill is a free read — nothing here spends the user's energy. Call
`get_energy_balance` if you need to report the balance.

## Paging

Every list reports how many rows it showed out of how many exist and states the offset for the
next page. A truncated table is never the whole answer — page before concluding.

## When to reach for a different skill

- **Account analytics** (`viralmaxing-analytics`) — Read metrics for the accounts a workspace tracks: views, engagement, VM Score (0-100 virality), outlier multiplier and per-video breakdowns over a period.
- **Competitor research** (`viralmaxing-research`) — Compare a creator against tracked competitors, discover new competitors in a niche, and find the formats already going viral by keyword, idea description or account.
- **Video intelligence** (`viralmaxing-video`) — Pull the transcript and metric detail of a specific short-form video, and export a set of videos as CSV.
- **Content plan** (`viralmaxing-plan`) — Read and update the content plan: turn a researched video into a scenario, save an edited script, and move scenarios between statuses.

## Not available here

Sending an Instagram Direct message is deliberately not exposed — the user sends those from the
app. Neither is account deletion. Viralmaxing does not publish or schedule posts to social
networks at all.

## More

- Developer portal: https://viralmaxing.com/developers
- Agent instructions: https://viralmaxing.com/agents.md
- Authentication walkthrough: https://viralmaxing.com/auth.md
- Product documentation: https://viralmaxing.com/docs (also served as markdown via `Accept: text/markdown`)
