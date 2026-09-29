# Repo Assist notes — fsprojects/fsharp-automation

## Repository shape
This repo is a fork/mirror of the F# compiler source. It has **no human-filed bug issues**.
All open issues are automation-generated:
- #1, #2 — Repo Assist monthly activity summaries (#2 is 2026-09)
- #3, #5 — `[aw] Detection Runs` trackers (do not touch)
- #4 — MSBuild File Quality Report (real, actionable findings)

No `AI-thinks-issue-fixed` or `AI-thinks-windows-only` labels exist in the repo, and those
labels are not in the repo's label list. Tasks 1–3 therefore have nothing to process.
Cursors `c`, `woc`, `rtc` stay at 0 for this reason — they are not stale.

## Issue #4 — ALL FIVE FINDINGS NOW VERIFIED. DO NOT RE-COMMENT.
Verified against HEAD `199945ab34433a3ec257e35e1fd30577a89ae350` (2026-08-28):
- DONE 2026-09-29 01:15: `_FSCorePackageVersionSet` dead write (ShimHelpers.props:37) — CONFIRMED
- DONE 2026-09-29 01:15: missing `@(FileWrites)` in `GenerateFSharpILLinkSubstitutions` — CONFIRMED
- DONE 2026-09-29 12:54: `CoreCompileDependsOn` overwrite (Microsoft.FSharp.Targets:224) — CONFIRMED
  with repro: `dotnet msbuild -getProperty:CoreCompileDependsOn` on .fsproj drops a
  Directory.Build.props contribution; .csproj keeps it. Nuance: VB overwrites too, only C#
  preserves. So it's a doc gap, not a clear bug.
- DONE 2026-09-29 12:54: unguarded shim imports — CONFIRMED, plus two extras: the `== ''`
  fallback imports are unreachable (ShimHelpers.props:29 always sets FSharpCompilerPath to
  the same dir), and the imports mix `/` and `\` separators.
- DONE 2026-09-29 01:15: `CreateManifestResourceNamesDependsOn` blanking (line 123) — noted as
  intentional C#/VB parity.

**Nothing left to verify on #4.** Only comment again if a human replies. Otherwise call noop.

## Standing conclusion
Absent new issues or a scope change, future runs should expect to call `noop`.
