# Viralmaxing — Agent Skill

Official [Agent Skill](https://skills.sh) for [Viralmaxing](https://viralmaxing.com): short-form content
intelligence for creators and brands. It teaches an agent how to read the analytics of the
Instagram / TikTok / YouTube accounts a workspace tracks, research viral formats, pull video
transcripts, and turn a researched video into a written script in the content plan.

## Install

```bash
npx skills add Viralmaxing/skill
```

## Connect

The skill drives the Viralmaxing MCP server over Streamable HTTP:

```
POST https://api.viralmaxing.com/api/mcp
```

Authenticate with OAuth 2.1 (dynamic client registration + PKCE — this is what Claude, Cursor
and Codex do when you add the connector by URL), or with an API key in the `X-API-Key` header.
Keys are created at https://viralmaxing.com/settings/api. A Viralmaxing account is required.

## What is in here

| Path | What it is |
|---|---|
| `skills/` | 5 skills, one per intent — see the table above |
| `plugin.json` | [Agent Plugins](https://agent-plugins.org) manifest |
| `mcp.json` | MCP server declaration for plugin hosts |
| `AGENTS.md` | Notes for AI coding agents working against this API |

## A note on the plan

38 of the 71 tools are reads. The
9 that produce something new need a paid plan and nothing else: no tool
has a price per call, and `confirm_cost` is not needed. What a plan limits is the accounts in
analytics across all of the owner's workspaces, the people on the team and the Instagram accounts
connected by login; an import takes a place in that limit, so name the accounts to the person
before calling.

## Generated, not hand-written

`SKILL.md` is generated from the connector's own tool catalog, so the tool list in it cannot
drift from the server that serves it. File issues about the skill's *content*
against the product; edits here would be overwritten on the next publish.

Docs: https://viralmaxing.com/developers · Product: https://viralmaxing.com

## License

MIT — see [LICENSE](./LICENSE).
