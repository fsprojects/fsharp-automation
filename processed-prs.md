# Regression PR Shepherd — processed PRs

| PR | Issue | Last checked (UTC) | Category | Notes |
|----|-------|--------------------|----------|-------|
| dotnet/fsharp#20663 | #5620 | 2026-09-30T21:00Z | Merged | Merged by T-Gro 2026-09-30T19:57Z. No further action. |
| dotnet/fsharp#20690 | #6310 | 2026-10-03T09:00Z | Merged | Merged by T-Gro 2026-10-02T06:27Z. No further action. |
| dotnet/fsharp#20648 | #2650 | 2026-10-06T01:20Z | Merged | CI green (build 1614286, 53/53). Zero review threads. mergeable_state=blocked (awaiting required maintainer review only). Last commit 1964aed by Shepherd 2026-09-28; no new activity since. |
| dotnet/fsharp#20671 | #5973 | 2026-10-06T01:20Z | Merged | CI green (build 1619004, 53/53). Zero review threads. No new activity since 2026-10-01T01:46Z. |

Both open PRs are healthy and awaiting maintainer review (abonie, T-Gro). No action taken in run 37267133639.

**Run 37222643065 (2026-10-04T18:01Z)**: Aborted — GitHub MCP server trapped (`module closed with context deadline exceeded`) on every call; `gh` unauthenticated. No PRs could be listed or triaged. Retry on next scheduled run.

**Run 37314159527 (2026-10-05T13:07Z)**: `search_pull_requests` for `repo:dotnet/fsharp is:open label:AI-Issue-Regression-PR` returned 0 results — consistent with run 37287678206 (09:06Z). #20648 and #20671 appear to have been merged/closed since 05:19Z. GitHub MCP server trapped (`module closed with context deadline exceeded`) on all subsequent calls, so no re-verification was possible. No action taken.

**Run 37398545408 (2026-10-06T01:20Z)**: 0 open PRs with `AI-Issue-Regression-PR` in dotnet/fsharp. Confirmed #20648 and #20671 were both merged by T-Gro on 2026-10-05T08:44Z / 08:43Z. Backlog empty; no action taken.

**Run 37467539400 (2026-10-06T13:02Z)**: 0 open PRs with `AI-Issue-Regression-PR` in dotnet/fsharp. Backlog still empty; no action taken.

**Run 37499824355 (2026-10-06T17:00Z)**: 1 open eligible PR — dotnet/fsharp#20706 (issue #6036, FCS namespace/module collision FS0247). Category B: 52/53 CI legs green, only `WindowsCompressedMetadata transparent_compiler_release` failed (build 1624907). Could NOT fetch AzDo logs — `dev.azure.com` blocked by agent firewall (CONNECT tunnel 403). Posted triage comment with hypothesis: test asserts `Array.exactlyOne` on project diagnostics; TransparentCompiler graph-based checking may not surface FS0247 (file 2 `namespace A.B` has no declared dep on file 1 `module B`), so the array is likely empty under TC. No fix pushed — refused to guess without the failing assertion text. Tagged @T-Gro @abonie. **Next run**: re-check #20706; if error text is available, apply targeted fix (dedicated non-TC `FSharpChecker` for this test, or TC-aware assertion).

**Run 37556804277 (2026-10-07T01:24Z)**: 1 open eligible PR — dotnet/fsharp#20706 (issue #6036). Category B3 **already fully handled by run 37530326702** (2026-10-06T21:29Z): failure identified as `System.ArgumentException : The input sequence was empty. (Parameter 'array')` from `Array.exactlyOne` under `TEST_TRANSPARENT_COMPILER=1` (`WindowsCompressedMetadata transparent_compiler_release`, build 1624907). Test passes with the default checker; TransparentCompiler does not surface FS0247 for the namespace/module collision. `AI-thinks-issue-fixed` confirmed removed from #6036; B3 comment posted and @T-Gro/@abonie tagged. **PR remains OPEN** — Shepherd has no `close_pull_request` safe output, so the final "close the PR" step cannot be performed by the agent. Zero review threads. No duplicate comment posted this run. **Next run**: skip #20706 unless new human review feedback or a new CI run appears; the ball is with maintainers (close manually, or accept a TC-tolerant assertion).

**Run 37574054828 (2026-10-07T05:00Z)**: 1 open eligible PR — dotnet/fsharp#20706 (issue #6036). No change since run 37556804277: PR `updated_at` still 2026-10-06T21:29:10Z (the B3 comment itself), zero review threads, no new CI run, `AI-thinks-issue-fixed` confirmed absent from #6036. Skipped per Step 3 (no new human feedback or CI results). PR still open — agent has no `close_pull_request` safe output. Ball remains with @T-Gro/@abonie.
