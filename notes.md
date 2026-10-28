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

## Issue #10 — HANDLED 2026-10-20 01:08. DO NOT RE-COMMENT.
**First human-filed issue in this repo** (mab2500, 2026-10-17): commands.yml still present
here after upstream deleted it. Verified ALL three claims at HEAD 199945a:
- `git hash-object .github/workflows/commands.yml` = 87edd6211f02f7d2febac343777ef6c902749f16
  (identical to upstream blob); workflow id 344815685, state=active, 0 runs.
- upstream commit 8ed69ca457dbd159863867ae12f65b37f89779aa = PR dotnet/fsharp#20786,
  merged by T-Gro 2026-10-15T10:28:47Z, changed_files 1 / deletions 236 / additions 0,
  empty PR body (no stated rationale). Path now 404s on dotnet/fsharp@main.
My two additions: (a) this clone's main is ~7wk behind upstream, so a routine sync carries
the deletion for free — a manual delete is redundant; (b) `.github/docs/state-machine.md`
row 6 is the ONLY other reference to commands.yml outside .git/ and must be updated if
hand-deleted. Did NOT open a PR — sync-vs-diverge is a maintainer call.
Only re-engage if a human replies.

## Issue #11 — ALL THREE FINDINGS VERIFIED 2026-10-21 12:51. DO NOT RE-COMMENT.
New msbuild-quality report (github-actions, 2026-10-21 05:04). Verified at HEAD 199945a,
SDK 10.0.402 / MSBuild 18.9.15.50611, Linux. Repro dir was /tmp/gh-aw/agent/repro.
- 🔴 @(CompileFirst)/@(CompileLast) item types declared in NetSdk.props:144-146,156-158 but
  FSharpSourceCodeCompileOrder (Targets:258-277) only reads the METADATA form. CONFIRMED.
  Repro A: <CompileFirst Include="a.fs"/> + <Compile Include="Program.fs"/> -> error FS0039.
  Repro B (MY ADDITION, not in issue): <Compile a.fs/> + <CompileLast Program.fs/> ->
  "Build succeeded." with ZERO diagnostics, Program.fs + [<EntryPoint>] silently dropped.
  Proof via target's own msg: `dotnet build -v:diag | grep FSharpSourceCodeCompileOrder:Sources`.
  Controls pass: CompileBefore item type, and Compile CompileOrder="CompileFirst" metadata.
  MY ADDITION 2: second ItemGroup lacks CompileFirst/CompileLast `Remove` lines -> double-splice
  risk if target runs twice. No PR opened (changes shipped-SDK ordering semantics = maintainer call;
  also the PR safe-output force-prefixes "Add regression test:" which doesn't fit).
- 🟡 shim D-4 CONFIRMED but issue OVERSTATES: both branches of the two NetSdk .targets shims
  import the SAME path (ShimHelpers.props:29 sets FSharpCompilerPath = Fsc_NetFramework_ToolPath
  = $(VsInstallRoot)/Common7/IDE/.../Tools/, identical to the hardcoded fallback). What is really
  lost: Fsc_NetFramework_PlatformSpecific_ToolExe, Fsc_DotNet_CompilerPath,
  Fsc_DotNET_DotnetFscCompilerPath, the Core.NetSdk.props import, FSCorePackageVersion fallback.
  NOT a duplicate of #4's shim item (that one is rule D-1, missing Exists() guards).
- 🔵 _FsGeneratedTfmAttributesSource (Targets:465) dead: grep has exactly 1 hit, the write.
Only re-engage if a human replies.

## Run log (short)

- **2026-10-02 .. 2026-10-27 (45+ runs, twice daily): all no-op except the four below.**
  Every run re-lists issues #1-#5,#7,#8,#10,#11 and PRs #6,#9 (both open drafts). Stamps frozen:
  #4 2026-09-29, #8 2026-10-14 12:53, #10 2026-10-20 01:12, #11/#7 2026-10-21 12:57,
  PR #6 2026-09-30, PR #9 2026-10-14. 14 labels; AI-thinks-* absent => Tasks 2-3 no input;
  Task 1 has no pre-2024 issues. Only #5 ([aw] Detection Runs) ticks each run - machine noise,
  do not touch. ZERO human comments anywhere, ever.
  Latest: 2026-10-28 00:55 UTC (8th consecutive quiet run; issue/PR updated_at stamps byte-identical
  to the 2026-10-27 12:54 run except #5 -> 2026-10-28 00:51. No issue #9/#12+; PRs still only #6/#9,
  both open drafts. #7 upd 2026-10-21, still 0 comments / 0 ticked checkboxes; 14 labels,
  AI-thinks-* still absent. noop; #7 NOT updated per Task FINAL rule 5; cursors c/woc/rtc=0,
  lr -> 2026-10-28.)
  Prior: 2026-10-27 12:54 UTC (7th consecutive quiet run; issue/PR updated_at stamps byte-identical
  to the 2026-10-27 01:11 run except #5 -> 2026-10-27 12:46. #7 still 0 comments / 0 ticked
  checkboxes; 14 labels, AI-thinks-* still absent; PRs still only #6/#9, both open drafts.
  noop; #7 NOT updated per Task FINAL rule 5; cursors c/woc/rtc=0, lr -> 2026-10-27.)
- 2026-10-14 12:49: new issue #8 + PR #9 (msbuild-quality) - duplicates of #4/#6. Commented on
  both; first update to summary #7.
- 2026-10-20 01:08: NEW HUMAN ISSUE #10 (mab2500). Verified + commented; updated #7.
  Needs-Triage label added (14 total).
- 2026-10-21 12:51: NEW ISSUE #11 (msbuild-quality). Verified all 3 findings, added 2 of my own
  + 1 correction. Commented; rewrote summary #7. Notable: #11 explicitly excluded findings
  already tracked in #4/#8, so msbuild-quality dedup may have improved since the #8/#9 incident.

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
- 2026-10-20 01:08 UTC: NEW HUMAN ISSUE #10 (mab2500, 2026-10-17) — first ever human-filed
  issue here. Verified and commented (see #10 section above). Updated monthly summary #7
  (added #10 action items + observation; prepended run entry). Other stamps unchanged:
  #4 2026-09-29, #8/#7 2026-10-14 12:53, PR #6 2026-09-30 (draft), PR #9 2026-10-14 (draft).
  Label set now 14 (Needs-Triage added); AI-thinks-* labels STILL absent => Tasks 2-3 no input.
  Only #5 ticks (machine noise).

- 2026-10-21 12:51 UTC: NEW ISSUE #11 (msbuild-quality, 2026-10-21 05:04). Verified all 3
  findings + added 2 of my own (silent CompileLast drop; missing Remove lines) and 1 correction
  (shim fallback path does NOT diverge). Commented on #11; rewrote monthly summary #7 (added #11
  items, prepended run entry, dropped the stale shim bullet). No new PRs (still only #6, #9,
  both open drafts). 14 labels, AI-thinks-* STILL absent => Tasks 2-3 no input; Task 1 has no
  pre-2024 issues. Notable: #11 explicitly excluded findings already tracked in #4/#8, so the
  msbuild-quality dedup logic may have improved since the #8/#9 duplicate incident.
