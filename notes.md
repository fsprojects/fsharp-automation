# Regression PR Shepherd — memory

Canonical files: `notes.md` (this) + `state.json`. Do not re-create history.jsonl /
processed-prs.* / shepherd-*.{txt,json} — they duplicated state.json and blew the 10 KB limit.

## Operational notes
- `gh` is NOT authenticated. Use GitHub MCP tools (`search_pull_requests`,
  `list_pull_requests`) for all reads. Fallback: unauthenticated
  `curl https://api.github.com/search/issues?q=...`. If both fail, `report_incomplete`.
- Search both `dotnet/fsharp` and `fsprojects/fsharp-automation` for the
  `AI-Issue-Regression-PR` label.
- The 3 long-lived open PRs in fsprojects/fsharp-automation (#6, #9, #12) are
  msbuild-quality drafts — permanently ineligible, skip silently.

## Run log (append one line per run; keep last ~15)
- 2026-10-31 run 41241197723 — 0 eligible PRs, noop.
- 2026-11-01 run 41259051199 — 0 eligible PRs, noop.
