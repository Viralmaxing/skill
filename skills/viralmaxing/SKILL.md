---
name: viralmaxing
description: "Analyze short-form video performance on Instagram Reels, TikTok and YouTube Shorts — a creator's own accounts and their competitors. Use when the user asks what is going viral in a niche or for an account, wants to read their own views / engagement / retention, wants a video transcript, wants to find competitors, or wants to turn a researched video into a written script in their content plan. Requires a Viralmaxing account."
---

# Viralmaxing

Short-form content intelligence: analytics for the Instagram / TikTok / YouTube accounts a
workspace tracks (the user's own and their competitors), viral-format research, video
transcripts, and a content plan where a researched video becomes a written script.

## When to use this skill

- Find out what is already going viral for a given Instagram / TikTok / YouTube account or in a niche — by account, keyword, or a description of the idea.
- Read a creator's own analytics: views, engagement, VM Score (0-100 virality), outlier multiplier, retention curve, per-video breakdown over a period.
- Compare a creator against tracked competitors — who is growing, which formats carry the growth, which posts are outliers.
- Pull the transcript of a short-form video and turn a researched post into a written scenario in the user's content plan (take_into_work).
- Read and update the content plan: list scenarios, read one, save an edited script, move it between statuses.
- Read Instagram comment/DM automation funnels and the leads they produced.
- Bring a new account under analysis: track an account the workspace does not have yet, discover competitors around it, or transcribe a specific video. These spend energy and require confirm_cost.

## When NOT to use it

- Publishing or scheduling posts — Viralmaxing does not post to social networks.
- Paid ad analytics, follower demographics, or DM/inbox management beyond the automation funnels.
- Video editing or rendering.
- A metric timeline for an account nobody tracks yet. `track_accounts` imports its recent videos on the spot (paid, per video), but day-by-day history only starts accumulating from that call onward.

## Connect

Everything runs over MCP (Streamable HTTP). One endpoint, two ways to authenticate:

```
POST https://api.viralmaxing.com/api/mcp
```

- **OAuth 2.1** — dynamic client registration + PKCE. Discovery document:
  `https://api.viralmaxing.com/.well-known/oauth-protected-resource`. This is what Claude, Cursor and
  Codex use when the user adds the connector by URL.
- **API key** — send `X-API-Key: vmx_...`. The user creates one at https://viralmaxing.com/settings/api.
  The same key works for the REST API at `https://api.viralmaxing.com/api` (OpenAPI: https://docs.viralmaxing.com/openapi.yaml).

Every tool is scoped to the authenticated user's workspace. There is no global or public data
surface — if the workspace does not track an account, no tool will return metrics for it.

## The loop

The tools are built around one path. Follow it rather than calling tools in isolation:

1. **Bring accounts in** — `track_accounts` for a known handle or URL, `discover_competitors`
   to find them around a seed, `find_ideas` to search by keyword or idea.
2. **Study them** — `list_posts`, `get_post_detail`, `get_transcript`,
   `create_account_report`, `get_account_overview`.
3. **Act** — `take_into_work` turns a post into a scenario (this is the canonical "act on this
   video" step, and it handles the transcript itself). `save_scenario` writes the script,
   `set_scenario_status` moves it through the plan.

Do NOT mass-transcribe posts to "prepare" them — `take_into_work` already transcribes what it
takes on, and transcription is billed.

## Money — the rule you must not break

Reading anything the account already holds is free. Producing something new costs energy: keyword search = 30; competitor discovery = 200; account report = 100; transcribing a video = 1 plus its measured cost; importing a tracked account = 1 per video (×2 on Facebook). Every paid tool REQUIRES a `confirm_cost` argument holding the exact price — state the cost to the user, get their agreement, then call. Call `get_energy_balance` if you need the balance first.

Paid tools reject a call whose `confirm_cost` is missing or wrong, and the refusal states the
real price. That refusal is not a retry signal: tell the user the number, get a yes, then call
again with the exact value.

## A worked example — the cost handshake

The paid path is the one worth getting right, so here it is end to end. The user says
"find me viral formats about morning routines".

1. Call it without guessing the price. The server tells you what it costs:

   ```json
   { "name": "find_ideas", "arguments": { "query": "morning routines" } }
   ```
   → `Not run — the cost must be confirmed first. This call costs 30 energy
   (one keyword search). ... call again with confirm_cost=30`

2. Tell the **user** that number and wait for a yes. Do not skip to step 3 on your own —
   the confirmation exists so a person approves the spend, not so the call succeeds.

3. Repeat with the exact value:

   ```json
   { "name": "find_ideas", "arguments": { "query": "morning routines", "confirm_cost": 30 } }
   ```
   → a session id. The search is now running.

4. Read the results with `get_idea_results`. Empty means "not yet" — poll, do not re-run
   step 3, which would charge again.

A wrong `confirm_cost` is refused the same way as a missing one, and the refusal always
states the real price — so you never have to guess it.

## Asynchronous tools

`track_accounts` and `transcribe_post` return an operation id — poll `get_operation`.
`find_ideas` and `discover_competitors` return a session id — read `get_idea_results` /
`get_discovery_results`. `create_account_report` is read back with `get_account_report`.

None of these finish inside the call that starts them. An empty result means "not yet", never
"run it again". Starting a second run duplicates the charge.

## Paging

Every list tool reports how many rows it showed out of how many exist and states the offset for
the next page. A truncated table is never the whole answer — page before concluding.

## Tools

30 tools: 20 read (free), 4 write, 6 that spend energy.

| Tool | What it does | Category |
|---|---|---|
| `create_account_report` | Analyze an account | spend |
| `discover_competitors` | Discover competitors | spend |
| `export_posts` | Export posts as CSV | read |
| `fetch` | Fetch a document | read |
| `find_ideas` | Search for ideas | spend |
| `get_account_overview` | Account overview | read |
| `get_account_report` | Account report | read |
| `get_automation_lead_thread` | Lead conversation | read |
| `get_automation_stats` | Automation results | read |
| `get_discovery_results` | Discovery results | read |
| `get_energy_balance` | Energy balance | read |
| `get_idea_results` | Search results | read |
| `get_operation` | Operation status | read |
| `get_post_detail` | Post detail | read |
| `get_scenario` | Scenario detail | read |
| `get_transcript` | Read a transcript | read |
| `list_automation_leads` | Automation leads | read |
| `list_automations` | Automations | read |
| `list_competitors` | Competitor accounts | read |
| `list_my_accounts` | My accounts | read |
| `list_posts` | Find posts | read |
| `list_scenarios` | Content plan | read |
| `save_automation_lead` | Record a lead outcome | write |
| `save_scenario` | Edit a scenario | write |
| `search` | Search | read |
| `set_scenario_status` | Move scenarios | write |
| `take_into_work` | Take a post into work | spend |
| `track_accounts` | Track accounts | spend |
| `track_discovered_competitor` | Track a discovered account | write |
| `transcribe_post` | Transcribe a video | spend |

## Not available here

Sending an Instagram Direct message is deliberately not exposed — the user sends those from the
app. Neither is account deletion. Publishing or scheduling posts to social networks is not part
of the product at all.

## More

- Developer portal: https://viralmaxing.com/developers
- Agent instructions: https://viralmaxing.com/agents.md
- Authentication walkthrough: https://viralmaxing.com/auth.md
- Product documentation: https://viralmaxing.com/docs (also served as markdown via `Accept: text/markdown`)
