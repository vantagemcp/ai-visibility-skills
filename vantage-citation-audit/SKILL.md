---
name: vantage-citation-audit
description: Check whether ChatGPT, Gemini or Perplexity cite a website, find out why a page is not cited, fix it and re-check. Use when the user asks "does AI cite my site", "why isn't this page cited", "check my AI visibility", "what does ChatGPT cite for this keyword", or wants to improve a page for AI answers. Needs the Vantage MCP server (vantagemcp.dev/mcp).
---

# Vantage citation audit

A four-step loop: measure, diagnose one page, change it, measure again. It uses the Vantage MCP tools, so
check they are available first. If they are not, tell the user to add `https://vantagemcp.dev/mcp` to their
MCP client (free tier, 30 units a month, no card) and stop.

## Before you spend anything

1. Call `get_usage` (0 units). Tell the user how many units remain.
2. Say what the audit will cost before running it. Rough costs:
   - `find_cited_questions`: 10 units
   - `analyze_citation_coverage`: 1 unit per keyword per sample (5 keywords x 3 samples = 15 units)
   - `analyze_citation_gap`: 1 unit per page
3. If the user has fewer units than the plan needs, cut keywords or samples and say so. Never silently shrink the
   check.

## 1. Measure

- If the user gave keywords, use them (up to 10). If not, ask for the domain and call `find_cited_questions`
  to see what AI already cites it for, then pick up to 5 questions a buyer would ask.
- Call `analyze_citation_coverage` with `samples=3` and the real brand name in `brand`. One answer is a sample,
  not a verdict: answers change from run to run.
- Report three things separately, never merged:
  - **cited**: an answer links the domain (`cited_runs` of `samples_ok`)
  - **mentioned, not cited**: the answer names the brand without linking it (`mentioned_not_cited`)
  - **not cited**: and who is cited instead (`source_frequency`)
- Say what changed since the last check if `previous` and `change` are present.

## 2. Diagnose one page

Pick the keyword the user most wants and one page that should answer it. Call `analyze_citation_gap` with that
keyword and the page URL. It returns `gaps`, `possibly_missing` and an ordered `fix_brief`.

- `possibly_missing` is word matching. Check each item against the page before treating it as missing.
- The last line of the brief is an off-site step. Report it: being cited is decided mostly by what other
  sites say about the brand, and no page rewrite replaces that.
- If the brief says no structural change is indicated, say so plainly. Do not invent edits.

## 3. Change the page

Carry out the `fix_brief` on the user's page, in the user's own words and from the user's own facts.

- Never copy wording from the cited answers. The tool does not return their text and you should not
  reconstruct it.
- Make the smallest change that follows the brief: usually the opening paragraph, missing sections, one
  table, or outbound sources. Show the diff before saving.

## 4. Measure again

After the user publishes the change, re-run step 1 with the same keywords, the same engine and the same
`samples`, so the result is directly comparable. Report `newly_cited`, `no_longer_cited` and `change` for each
keyword. Do this after the page is live, and allow time for engines to pick it up: a re-check the same day will
usually show no change.

## Rules

- Be honest about uncertainty: say "cited in 2 of 3 answers", not "cited".
- Do not promise a result. A fix brief is a hypothesis about page structure.
- Use `engine="gemini"` or `"perplexity"` only when the user asks; they cost the same but answer differently.
- Other markets: pass `country` and `language`; `get_supported_options` lists what each tool accepts.
