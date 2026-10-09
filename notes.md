# Repo Assist notes — fsprojects/fsharp-automation

## Repository shape
This repo is a fork/mirror of the F# compiler source. It has **no human-filed bug issues**.
All open issues are automation-generated:
- #1, #2, #7 — Repo Assist monthly activity summaries. **#7 is the live 2026-10 one**
  (`ms` = 7). #1 (2026-08) and #2 (2026-09) are superseded; a maintainer should close them.
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
- 2026-10-02 .. 2026-10-09 (14 runs, twice daily): every run re-listed all issues (#1-#5, #7)
  and all PRs (only #6, still draft). No new issues, no new PRs, zero human comments anywhere.
  Stamps frozen at #4 2026-09-29, PR #6 2026-09-30, #7 2026-10-01. Repo labels re-listed
  several times (13 labels): `AI-thinks-issue-fixed` / `AI-thinks-windows-only` still absent,
  so Tasks 1-3 have no input. Only #5 ([aw] Detection Runs) ticks each run — machine noise,
  do not touch. Every run -> noop; monthly summary #7 NOT updated (no activity).
- 2026-10-09 12:52 UTC: same as above. noop.

## Standing conclusion
Absent new issues/PRs or a scope change, future runs should expect to call `noop`.
Before noop-ing, always check for NEW PRs (not just issues) — PR #6 was missed on the
prior run because only issues were listed.

## Why no PR is opened for the verified #4 findings
The `safe-outputs.create-pull-request` config for this workflow force-prefixes titles with
"Add regression test: " and force-adds `NO_RELEASE_NOTES` + `AI-Issue-Regression-PR`. That
shape only fits Task 2 regression-test PRs. Opening the one-line `@(FileWrites)` fix for
`GenerateFSharpILLinkSubstitutions` through it would ship a mislabelled, misleadingly-titled
PR touching `src/`. Leave it to a human (it is already listed in summary #7).

