# AGENTS.md

Notes for AI coding agents integrating Viralmaxing.

## Prefer MCP over REST

The MCP server at `https://api.viralmaxing.com/api/mcp` is the supported surface, and the skills under `skills/`
document it. Reach for the REST API only when you need something MCP does not expose; it is described by
an OpenAPI 3.0 spec linked from https://viralmaxing.com/developers.

## Authentication

One credential covers both rails: an API key created at https://viralmaxing.com/settings/api, sent as
`X-API-Key: vmx_...`. MCP additionally supports OAuth 2.1 with dynamic client registration, which
is what interactive clients use. Never hardcode a key into a repository — read it from the
environment.

## Rules that will save you a support ticket

- **Mind the plan's limits.** No tool has a price per call and `confirm_cost` is not needed. A
  call past the plan's accounts, team or Instagram limit is refused with the limit — surface it to
  the human; do not retry, and do not import accounts the human has not named.
- **Poll, do not re-run.** Imports, transcriptions, searches and discovery are asynchronous. An
  empty result means "not ready", and starting a second run does the same work twice.
- **Page.** Every list reports how many rows it showed out of how many exist, and the offset for
  the next page.
- **Everything is workspace-scoped.** An account's own metrics come back only when the workspace
  tracks it. What reaches past the workspace is the keyword search over the whole video base and
  the niches — shared lists of authors with their followers and usual views.

## Discovery documents

- `https://viralmaxing.com/llms.txt` — what the product is for, and when not to use it
- `https://viralmaxing.com/agents.md` — the same guidance for agent hosts
- `https://viralmaxing.com/auth.md` — authentication walkthrough
- `https://viralmaxing.com/.well-known/api-catalog` — RFC 9727 linkset naming every machine-readable document
