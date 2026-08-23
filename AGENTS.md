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

- **Confirm costs.** Paid tools reject a call without an exact `confirm_cost`. That refusal names
  the price. Do not retry with a guess — surface the number to the human first.
- **Poll, do not re-run.** Imports, transcriptions, searches and discovery are asynchronous. An
  empty result means "not ready", and starting a second run charges a second time.
- **Page.** Every list reports how many rows it showed out of how many exist, and the offset for
  the next page.
- **Everything is workspace-scoped.** There is no public data surface; if the workspace does not
  track an account, no call will return metrics for it.

## Discovery documents

- `https://viralmaxing.com/llms.txt` — what the product is for, and when not to use it
- `https://viralmaxing.com/agents.md` — the same guidance for agent hosts
- `https://viralmaxing.com/auth.md` — authentication walkthrough
- `https://viralmaxing.com/.well-known/api-catalog` — RFC 9727 linkset naming every machine-readable document
