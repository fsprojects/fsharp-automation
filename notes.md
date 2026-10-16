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

## Issues #8 / PR #9 — HANDLED 2026-10-14 12:49. DO NOT RE-COMMENT.
msbuild-quality workflow re-emitted the ValueTupleImplicitPackageVersion finding as "new":
issue #8 + PR #9. PR #9 diff == PR #6 diff except whitespace in the Condition attribute.
Commented on both: duplicate, plus corrected their evaluation-order rationale.
Re-verified at HEAD 199945a (MSBuild 18.9.11 / SDK 10.0.401): evaluation is sequential over
the flattened import tree, so a PROJECT-BODY assignment survives (7.7.7) while a
Directory.Build.props one is discarded (4.6.2). "No way to pin" is overstated; the real bug
is only the pre-import sources. SetImplicitReferenceTest.proj sets it in the body -> not failing.
Only re-engage if a human replies.

## Run log (short)
- 2026-10-02 .. 2026-10-11 (18+ runs, twice daily): every run re-listed all issues (#1-#5, #7)
  and all PRs (only #6, still draft). No new issues, no new PRs, zero human comments anywhere.
  Stamps frozen at #4 2026-09-29, PR #6 2026-09-30, #7 2026-10-01. 13 repo labels unchanged;
  `AI-thinks-issue-fixed` / `AI-thinks-windows-only` still absent, so Tasks 1-3 have no input.
  Only #5 ([aw] Detection Runs) ticks each run — machine noise, do not touch.
  Every run -> noop; monthly summary #7 NOT updated (no activity).
- 2026-10-12 01:26 UTC: issues (#1-#5,#7), PRs (only #6, draft, updated 2026-09-30) and the
  13 labels re-verified — all stamps unchanged vs. previous run. Only #5 ticked
  (2026-10-12 01:19). noop; summary #7 not updated.
- 2026-10-12 13:13 UTC: re-verified issues (#1-#5,#7), PRs (only #6, draft, 2026-09-30) and
  the 13 labels. All stamps identical to the 01:26 run; #4 still 2026-09-29, #7 still
  2026-10-01. Only #5 ticked (2026-10-12 13:04). Zero human comments anywhere. noop.
- 2026-10-13 01:21 and 12:57 UTC: re-verified issues (#1-#5,#7), PRs (only #6, draft,
  2026-09-30) and the 13 labels. All stamps identical to the 2026-10-12 13:13 run; #4 still
  2026-09-29, #7 still 2026-10-01, #7 has zero comments. Only #5 ticked (2026-10-13 12:49).
  Zero human comments anywhere. noop.
- 2026-10-14 01:07 UTC: re-verified issues (#1-#5,#7) and PRs (only #6, open, draft,
  updated 2026-09-30). All stamps identical to the 2026-10-13 runs; #4 2026-09-29,
  #7 2026-10-01. Only #5 ticked (2026-10-14 01:01). Zero human comments anywhere. noop.

- 2026-10-16 01:05 UTC: re-verified issues (#1-#5,#7,#8) and PRs (#6, #9 — both still open drafts).
  Every stamp identical to the 2026-10-15 12:51 run: #4 upd 2026-09-29, #8 upd 2026-10-14 12:53,
  #7 upd 2026-10-14 12:53, PR #6 upd 2026-09-30, PR #9 upd 2026-10-14 12:53. Only #5 ([aw]
  Detection Runs) ticked (2026-10-16 00:59) — machine noise. No AI-thinks-* labels exist, so
  Tasks 1-3 have no input. Zero human comments anywhere. noop; summary #7 not updated.
- 2026-10-15 12:51 UTC: re-verified issues (#1-#5,#7,#8) and PRs (#6, #9 — both still open drafts).
  Every stamp identical to the 2026-10-15 01:06 run: #4 upd 2026-09-29, #8 upd 2026-10-14 12:53,
  #7 upd 2026-10-14 12:53, PR #6 upd 2026-09-30, PR #9 upd 2026-10-14 12:53. Only #5 ([aw]
  Detection Runs) ticked (2026-10-15 12:43) — machine noise. No AI-thinks-* labels exist, so
  Tasks 1-3 have no input. Zero human comments anywhere. noop; summary #7 not updated.

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
- 2026-10-15 01:06 UTC: re-verified issues (#1-#5,#7,#8) and PRs (#6 and #9, both open drafts).
  Everything identical to the 2026-10-14 12:49 run: #4 2 comments (both mine, 2026-09-29),
  #8 1 comment (mine, 2026-10-14 12:53), #7 still 0 comments, PR #9 upd 2026-10-14 12:53,
  PR #6 upd 2026-09-30. 13 labels unchanged, no AI-thinks-* labels => Tasks 1-3 no input.
  Zero human comments anywhere. noop; summary #7 not updated (no activity this run).
- 2026-10-14 12:49 UTC: FIRST new activity in ~2 weeks. New issue #8 and new PR #9 (both
  github-actions[bot], msbuild-quality, 2026-10-14 05:03/05:04) — duplicates of #4/#6.
  Commented on #8 and #9; updated monthly summary #7 (first update since creation).
  Still zero human comments anywhere. #1-#5 stamps unchanged.
