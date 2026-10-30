# Regression PR Shepherd — notes

Single source of truth: `state.json` (processed PRs) + `shepherd-runs.txt` (last ~8 runs only).
Do not add new log files — total memory must stay under ~10 KB.

## Recurring infra issue
The GitHub MCP server sometimes traps on the first call ("WASM guard 'github' is unavailable
after a previous trap"). `gh` is unauthenticated.
Fallback that has worked: unauthenticated `curl https://api.github.com/search/issues?q=...`.
If curl is also blocked, call `report_incomplete`.
