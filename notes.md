# Regression PR Shepherd — notes

Consolidated 2026-10-29. Single source of truth: `state.json` (processed PRs) + `shepherd-runs.txt` (run log).

## Recurring infra issue
The GitHub MCP server frequently traps on the first call
("WASM guard 'github' is unavailable after a previous trap" / "context deadline exceeded")
and never recovers. `gh` is unauthenticated, so there is no fallback read path via gh.

**Fallback that has worked:** unauthenticated
`curl https://api.github.com/search/issues?q=...` returns public data fine.
Try this first when MCP traps. (Note: in some sandboxes the firewall blocks api.github.com;
if curl also fails, call `report_incomplete`.)
