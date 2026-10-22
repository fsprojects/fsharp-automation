
## Run 38904475876 (2026-10-16)
GitHub MCP server trapped ("WASM guard 'github' is unavailable after a previous trap") on the
very first call and never recovered after 90s of retries. `gh` is unauthenticated, so there was
no fallback read path. No PRs could be listed or processed this run.

## Run 39751850752 (2026-10-22)
GitHub MCP server trapped again on first call and never recovered (~3 min of retries).
Fallback that worked: unauthenticated `curl https://api.github.com/search/issues?q=...` returns
public data fine. Used it to confirm 0 open `AI-Issue-Regression-PR` PRs in dotnet/fsharp.
Recommend trying this curl fallback first in future runs when MCP traps.
