# Repo Assist notes — fsprojects/fsharp-automation

## Repository shape
This repo is a fork/mirror of the F# compiler source. It has **no human-filed bug issues**.
All open issues are automation-generated:
- #1, #2 — Repo Assist monthly activity summaries (#2 is 2026-09)
- #3 — `[aw] Detection Runs` tracker (do not touch)
- #4 — MSBuild File Quality Report (has real, actionable findings)

No `AI-thinks-issue-fixed` or `AI-thinks-windows-only` labels exist in the repo, and those
labels are not in the repo's label list. Tasks 1–3 therefore have nothing to process.
Cursors `c`, `woc`, `rtc` stay at 0 for this reason — they are not stale.

## Issue #4 verification progress
Verified against HEAD `199945ab34433a3ec257e35e1fd30577a89ae350` (2026-08-28):
- DONE 2026-09-29: `_FSCorePackageVersionSet` dead write (vsintegration/shims/Microsoft.FSharp.ShimHelpers.props:37) — CONFIRMED
- DONE 2026-09-29: missing `@(FileWrites)` in `GenerateFSharpILLinkSubstitutions` (src/FSharp.Build/Microsoft.FSharp.NetSdk.targets:213-219) — CONFIRMED
- TODO: `CoreCompileDependsOn` overwrite in src/FSharp.Build/Microsoft.FSharp.Targets ~line 224
- TODO: unguarded (no `Exists()`) shim imports in vsintegration/shims/*.targets, *.props

Do not re-comment on #4 with the same two findings. Only comment again if a human replies
or if the two TODO findings above are verified.
