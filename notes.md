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

## Issue #4 — ALL FIVE FINDINGS VERIFIED. DO NOT RE-COMMENT.
Verified against HEAD `199945ab34433a3ec257e35e1fd30577a89ae350` (2026-08-28):
- `_FSCorePackageVersionSet` dead write (ShimHelpers.props:37) — CONFIRMED
- missing `@(FileWrites)` in `GenerateFSharpILLinkSubstitutions` — CONFIRMED (also no Inputs/Outputs)
- `CoreCompileDependsOn` overwrite (Microsoft.FSharp.Targets:224) — CONFIRMED with repro.
  VB overwrites too; only C# preserves. Doc gap, not a clear bug.
- unguarded shim imports — CONFIRMED, plus `== ''` fallback imports unreachable, and
  imports mix `/` and `\` separators.
- `CreateManifestResourceNamesDependsOn` blanking (line 123) — intentional C#/VB parity.

**Nothing left to verify on #4.** Only comment again if a human replies.

## PR #6 — REVIEWED 2026-09-30 12:56. DO NOT RE-COMMENT.
Draft PR from msbuild-quality workflow: guards `ValueTupleImplicitPackageVersion`
(Microsoft.FSharp.NetSdk.props:106, unconditional assignment).
Verified: `Directory.Build.props` value IS discarded (-> 4.6.2), but a **project-body**
assignment DOES survive (-> 7.7.7), because `Sdk="..."` imports SDK props at the top.
So the PR body's "no way to pin" sentence is overstated. Commented with the repro.
Only re-engage if a human replies or the PR body changes.

## Run log (short)
- 2026-09-30 01:18 UTC: rescanned, nothing new. noop.
- 2026-09-30 12:56 UTC: commented on PR #6 with verification repro; updated summary #2.

## Standing conclusion
Absent new issues/PRs or a scope change, future runs should expect to call `noop`.
Before noop-ing, always check for NEW PRs (not just issues) — PR #6 was missed on the
prior run because only issues were listed.
