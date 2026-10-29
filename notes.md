# Regression PR Shepherd — notes

Single source of truth: `shepherd-state.json` (last run + processed PRs) + `shepherd-runs.txt` (last ~8 runs only; trim to stay under the 10 KB memory patch limit).

## Recurring infra issue
The GitHub MCP server sometimes traps on the first call ("WASM guard 'github' is unavailable
after a previous trap" / "context deadline exceeded") and never recovers. `gh` is unauthenticated.

**Fallback that has worked:** unauthenticated `curl https://api.github.com/search/issues?q=...`.
If curl is also firewall-blocked, call `report_incomplete`.

## Memory hygiene
Total memory must stay under ~10 KB. Do not add new log files; append one line per run to
`shepherd-runs.txt` and trim it to the last 8 lines.
