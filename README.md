# Vantage skills

Agent skills for checking and improving how often ChatGPT, Gemini and Perplexity cite a site. They use the
[Vantage MCP server](https://github.com/vantagemcp/vantage-mcp), which is free to start (30 units a month, no
card).

| Skill | What it does |
|---|---|
| [`vantage-citation-audit`](vantage-citation-audit/SKILL.md) | Measure which keywords cite a domain, diagnose one page, apply the fix brief, re-check |

## Install

1. Add the MCP server: `claude mcp add --transport http vantage https://vantagemcp.dev/mcp`
2. Copy `vantage-citation-audit/` into your agent's skills folder (for Claude Code, `~/.claude/skills/`).

Then ask your agent: "Run a Vantage citation audit on example.com."

## Notes

- Skills only describe the workflow; every check is a Vantage tool call and spends units from your quota.
- Answers change from run to run, so the skill reads three samples per keyword.

MIT licensed.
