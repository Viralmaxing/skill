---
name: viralmaxing-video
description: "Understand one specific video: its full metrics, its caption, and what is actually said in it, via transcription. Use when the user says things like: \"transcribe this Reel\", \"what does this video say\", \"show me the numbers on this post\", \"why did this one work\" или по-русски: «расшифруй этот рилс», «о чём это видео», «покажи метрики этого поста», «почему этот ролик залетел». Requires a Viralmaxing account."
---

# Video intelligence

Understand one specific video: its full metrics, its caption, and what is actually said in it, via transcription.

## When to use this skill

- "transcribe this Reel"
- "what does this video say"
- "show me the numbers on this post"
- "why did this one work"

По-русски: «расшифруй этот рилс», «о чём это видео», «покажи метрики этого поста», «почему этот ролик залетел».

## Connect

Everything runs over MCP (Streamable HTTP):

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
| `get_post_detail` | Post detail | read |
| `get_transcript` | Read a transcript | read |
| `transcribe_post` | Transcribe a video | spend |

Available from every Viralmaxing skill: `get_energy_balance`, `get_operation`, `search`, `fetch`.

## Cost — the rule you must not break

- `transcribe_post` — 1 energy plus the video's measured cost

Every one of these **requires** a `confirm_cost` argument holding the exact price. The server
refuses the call without it and states the real number; that refusal is not a retry signal.

The handshake, end to end:

1. Call without guessing the price. The refusal names it.
2. Tell the **user** that number and wait for a yes. The confirmation exists so a person
   approves the spend, not so the call succeeds.
3. Call again with `"confirm_cost": <the exact number>`.

Reading anything the workspace already holds is free. `get_energy_balance` reports what is left.

## Asynchronous tools

- `transcribe_post` returns an operation id — poll `get_operation`

None of these finish inside the call that starts them. An empty result means "not yet", never
"run it again" — a second run charges a second time.

## Paging

Every list reports how many rows it showed out of how many exist and states the offset for the
next page. A truncated table is never the whole answer — page before concluding.

## When to reach for a different skill

- **Account analytics** (`viralmaxing-analytics`) — Read metrics for the accounts a workspace tracks: views, engagement, VM Score (0-100 virality), outlier multiplier and per-video breakdowns over a period.
- **Competitor research** (`viralmaxing-research`) — Compare a creator against tracked competitors, discover new competitors in a niche, and find the formats already going viral by keyword, idea description or account.
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
