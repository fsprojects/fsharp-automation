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
- 2026-09-30 01:18 UTC: rescanned, nothing new. noop.
- 2026-09-30 12:56 UTC: commented on PR #6 with verification repro; updated summary #2.
- 2026-10-01 01:25 UTC: month rollover. No new issues/PRs/human comments. Created the
  2026-10 monthly summary (= issue #7), carrying forward the unactioned items from #2.
- 2026-10-01 12:55 UTC: resolved `ms` to 7. Re-listed all issues AND PRs: still only #1-#7
  and PR #6, every one authored by github-actions[bot]. Zero human comments exist anywhere
  in the repo. #4 and #6 unchanged since my last comments on them. Nothing to do -> noop,
  and per the workflow rules the monthly summary was NOT updated (no activity this run).
- 2026-10-02 01:15 UTC: re-listed all issues (#1-#5, #7) and all PRs (only #6). Every item
  still authored by github-actions[bot]; re-read #4 and #7 comment threads in full — zero
  human comments exist anywhere in the repo. #4 unchanged since 2026-09-29, PR #6 unchanged
  since 2026-09-30. #5 ([aw] Detection Runs) ticks every run; do not touch. Nothing to do
  -> noop, and the monthly summary #7 was NOT updated (no activity this run).
- 2026-10-02 12:53 UTC: re-listed all issues (#1-#5, #7) and all PRs (only #6). No new
  issues, no new PRs, zero human comments anywhere. #4 last touched 2026-09-29, PR #6
  2026-09-30, #7 2026-10-01 — all unchanged since my previous comments. Nothing to do
  -> noop; monthly summary #7 NOT updated (no activity this run).
- 2026-10-03 01:12 UTC: re-listed all issues (#1-#5, #7) and all PRs (only #6, still draft).
  No new issues, no new PRs, zero human comments anywhere in the repo. Last-updated stamps:
  #4 2026-09-29, PR #6 2026-09-30, #7 2026-10-01 — all predate `lr`, so nothing changed since
  my previous engagement. Only #5 ([aw] Detection Runs) ticked, which is machine noise — do not
  touch. Nothing to do -> noop; monthly summary #7 NOT updated (no activity this run).

- 2026-10-03 14:17 UTC: re-listed all issues (#1-#5, #7) and all PRs (only #6, still draft).
  No new issues, no new PRs, zero human comments anywhere. Last-updated stamps: #4 2026-09-29,
  PR #6 2026-09-30, #7 2026-10-01 — all predate this run and my previous engagement. Only #5
  ([aw] Detection Runs, 2026-10-03 12:46) ticked — machine noise, do not touch. Nothing to do
  -> noop; monthly summary #7 NOT updated (no activity this run).

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

