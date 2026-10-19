# Vantage skills

Agent skills for checking and improving how often ChatGPT, Gemini and Perplexity cite a site. They use the
[Vantage MCP server](https://github.com/vantagemcp/vantage-mcp), which is free to start (30 units a month, no
card).

| Skill | What it does |
|---|---|
| [`vantage-citation-audit`](skills/vantage-citation-audit/SKILL.md) | Measure which keywords cite a domain, diagnose one page, apply the fix brief, re-check |

This repository is a Claude Code plugin: `.claude-plugin/plugin.json` describes it, `.mcp.json` points at the hosted
Vantage server (`https://vantagemcp.dev/mcp`, sign in with your email on first use), and `skills/` holds the skill.

## Install

**As a Claude Code plugin** (also adds the MCP server):

```
/plugin marketplace add vantagemcp/ai-visibility-skills
/plugin install vantage-ai-visibility@vantage
```

**By hand:**

1. Add the MCP server: `claude mcp add --transport http vantage https://vantagemcp.dev/mcp`
2. Copy `skills/vantage-citation-audit/` into your agent's skills folder (for Claude Code, `~/.claude/skills/`).

Then ask your agent: "Run a Vantage citation audit on example.com."

## What the plugin sends and stores

The plugin itself runs no code and holds no credentials. Every check is a call to the Vantage server, which sends
the keyword or domain you check to its data provider and keeps coverage results against your key for 180 days. See
the [privacy policy](https://vantagemcp.dev/legal/?ref=gh-skills).

## Notes

- Skills only describe the workflow; every check is a Vantage tool call and spends units from your quota.
- Answers change from run to run, so the skill reads three samples per keyword.

MIT licensed.
