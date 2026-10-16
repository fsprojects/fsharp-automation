
## Run 38904475876 (2026-10-16)
GitHub MCP server trapped ("WASM guard 'github' is unavailable after a previous trap") on the
very first call and never recovered after 90s of retries. `gh` is unauthenticated, so there was
no fallback read path. No PRs could be listed or processed this run.
