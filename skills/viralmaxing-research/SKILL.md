---
name: viralmaxing-research
description: "Find what is already working: the formats going viral for a keyword, an idea or an account, and the competitors worth watching in a niche. Use when the user says things like: \"what is working for my competitors right now\", \"find viral formats about morning routines\", \"who else is big in this niche\", \"find me competitors\" или по-русски: «что заходит у конкурентов», «найди вирусные форматы про утренние привычки», «кто ещё крупный в этой нише», «подбери конкурентов». Reach for this skill whenever competitors, niches, trends, or what format is working on short-form video right now comes up, even if the user never says \"Viralmaxing\". Requires a Viralmaxing account and the Viralmaxing MCP server connected (https://api.viralmaxing.com/api/mcp) — without it none of these tools are callable."
---

# Competitor research

Find what is already working: the formats going viral for a keyword, an idea or an account, and the competitors worth watching in a niche.

## When to use this skill

- "what is working for my competitors right now"
- "find viral formats about morning routines"
- "who else is big in this niche"
- "find me competitors"

По-русски: «что заходит у конкурентов», «найди вирусные форматы про утренние привычки», «кто ещё крупный в этой нише», «подбери конкурентов».

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
| `list_competitors` | Competitor accounts | read |
| `discover_competitors` | Discover competitors | spend |
| `get_discovery_results` | Discovery results | read |
| `track_discovered_competitor` | Track a discovered account | write |
| `find_ideas` | Search for ideas | spend |
| `get_idea_results` | Search results | read |

Available from every Viralmaxing skill: `get_energy_balance`, `get_operation`, `search`, `fetch`.

## Cost — the rule you must not break

- `discover_competitors` — 200 energy per discovery run
- `find_ideas` — 30 energy per keyword search

Every one of these **requires** a `confirm_cost` argument holding the exact price. The server
refuses the call without it and states the real number; that refusal is not a retry signal.

The prices above come from the same catalog the server bills from, so there is no need to probe
for them:

1. Tell the **user** the number and wait for a yes. The confirmation exists so a person approves
   the spend, not so the call succeeds.
2. Call with `"confirm_cost": <the exact number>`.

If you are ever unsure of a price, calling without `confirm_cost` is safe — the server refuses
and quotes the real number rather than charging. Use that as a fallback, not as step one.

**The price is per call, not per result.** A search costs the same whether it returns 20 rows or
100, so ask for a generous limit the first time instead of paying twice for a second pass.

Reading anything the workspace already holds is free. `get_energy_balance` reports what is left.

## Asynchronous tools

- `discover_competitors` returns a session id — read `get_discovery_results`
- `find_ideas` returns a session id — read `get_idea_results`

None of these finish inside the call that starts them. An empty result means "not yet", never
"run it again" — a second run charges a second time.

## Paging

Every list reports how many rows it showed out of how many exist and states the offset for the
next page. A truncated table is never the whole answer — page before concluding.

## When to reach for a different skill

- **Account analytics** (`viralmaxing-analytics`) — Read metrics for the accounts a workspace tracks: views, engagement, VM Score (0-100 virality), outlier multiplier and per-video breakdowns over a period.
- **Video intelligence** (`viralmaxing-video`) — Pull the transcript and metric detail of a specific short-form video, and export a set of videos as CSV.
- **Content plan** (`viralmaxing-plan`) — Read and update the content plan: turn a researched video into a scenario, save an edited script, and move scenarios between statuses.
- **Instagram automation funnels** (`viralmaxing-automations`) — Read Instagram comment and Direct automation rules, their delivery stats, and the leads they produced.

## Not available here

Sending an Instagram Direct message is deliberately not exposed — the user sends those from the
app. Neither is account deletion. Viralmaxing does not publish or schedule posts to social
networks at all.

## More

- Developer portal: https://viralmaxing.com/developers
- Agent instructions: https://viralmaxing.com/agents.md
- Authentication walkthrough: https://viralmaxing.com/auth.md
- Product documentation: https://viralmaxing.com/docs (also served as markdown via `Accept: text/markdown`)
