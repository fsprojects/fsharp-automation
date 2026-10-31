# Repo Assist notes — fsprojects/fsharp-automation

## Repository shape
Fork/mirror of the F# compiler source. Almost no human-filed issues; nearly all are
automation-generated. #1/#2/#7 = Repo Assist monthly summaries (**#7 is live, `ms`=7**; #1,#2
superseded, maintainer should close). #3/#5 = `[aw] Detection Runs` trackers (do not touch).
#4/#8/#11/#13 = MSBuild File Quality Reports. AI-thinks-issue-fixed / AI-thinks-windows-only
labels do NOT exist in this repo => Tasks 1-3 have nothing to process; cursors c/woc/rtc stay 0
(not stale).

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
Verified at HEAD 199945a, SDK 10.0.402, Linux.
- CompileFirst/CompileLast declared (NetSdk.props:144-146,156-158) but FSharpSourceCodeCompileOrder
  (Targets:258-277) reads only the METADATA form. Repro A: <CompileFirst a.fs/> -> error FS0039.
  Repro B (MY ADDITION): <CompileLast Program.fs/> -> "Build succeeded." ZERO diagnostics,
  [<EntryPoint>] silently dropped. Proof: dotnet build -v:diag | grep FSharpSourceCodeCompileOrder:Sources.
  MY ADDITION 2: second ItemGroup lacks CompileFirst/CompileLast Remove lines -> double-splice risk.
- shim D-4 CONFIRMED but OVERSTATED: both branches import the SAME path (ShimHelpers.props:29
  FSharpCompilerPath == the hardcoded fallback). What's lost: Fsc_NetFramework_PlatformSpecific_ToolExe,
  Fsc_DotNet_CompilerPath, Fsc_DotNET_DotnetFscCompilerPath, Core.NetSdk.props import,
  FSCorePackageVersion fallback. NOT a dup of #4's shim item (that's D-1 missing Exists()).
- _FsGeneratedTfmAttributesSource (Targets:465) dead. (#13 later restated this -> dedup regression.)
- STALE/WRONG in #11: it declared _CheckForUnsupportedFSharpCoreVersion "sound". #13/PR#12 disproved it.
No PR opened (shipped-SDK ordering semantics = maintainer call; PR safe-output force-prefixes
"Add regression test:" which doesn't fit). Only re-engage if a human replies.


## Issue #13 / PR #12 — HANDLED 2026-10-28 12:41. DO NOT RE-COMMENT.
New msbuild-quality report #13 + companion draft PR #12 (both github-actions, 2026-10-28 05:06).
Verified at HEAD 199945a, SDK 11.0.100-preview.6.26359.118 / MSBuild 18.9.0.36018, Linux.
Probes in /tmp/gh-aw/agent/probe (a=unguarded parse, b=patched parse, c=batching, d=de-batch).
ALL FIVE #13 findings CONFIRMED technically, plus my 3 additions/corrections:
- F1 Fsc task-batching on %(_CoreCompileResourceInputs.WithCulture): CONFIRMED.
  Probe c: stray item w/o WithCulture -> TWO byte-identical invocations. Empty group -> 1 (benign).
  MY CORRECTION: #13's "culture resources never removed from what is handed to the compiler" is
  WRONG. SDK Microsoft.Common.CurrentVersion.targets:3698/3699 filter WithCulture=='false';
  3702/3706 explicitly stamp <WithCulture>false</WithCulture>. All 4 sites => _CoreCompileResourceInputs
  is culture-free by construction. Condition is a vestigial XBuild-era no-op, not a live bug.
  MY ADDITION: verified de-batching fix (property hoist with WithMetadataValue), 1 invocation in all
  4 culture/stray combos, same Resources= lists. Cheaper than #13's "needs resource-heavy test project".
- F2 _FsGeneratedTfmAttributesSource dead: CONFIRMED (grep 1 hit) but EXACT DUPLICATE of #11's
  blue "Dead item write", despite #13 claiming it restates nothing from #11. Dedup REGRESSION
  (#11 had deduped correctly vs #4/#8).
- F3 _CheckForUnsupportedFSharpCoreVersion: CONFIRMED. Probe a reproduced
  "error MSB4184 ... [System.Version]::Parse('')". Count()==1 constrains item count not metadata.
  MY CORRECTION: it's a TARGET-EXECUTION error (AfterTargets=ResolvePackageAssets), NOT
  "project fails to evaluate" as both #13 and PR #12 say. Restore/design-time builds unaffected.
  NOTABLE: #13 REVERSES #11, which declared this target "sound ... both operands proven non-empty
  by the Count()==1 guard". #13 is right, #11 was wrong. Flagged on both.
- S4 LangVersion hardcode (NetSdk.props:101-103): CONFIRMED. MY CORRECTION: pinned in TWO places
  (value AND StartsWith('11.') in condition); #13's fix only changes one => still dormant on SDK 12.
  Also LangVersion is an F# language version, separate numbering from SDK version.
- S5 GenerateFSharpTextResources (Targets:227-228) no Inputs/Outputs: CONFIRMED.
PR #12 verified GOOD: probe b, 4 versions. empty->no crash/no warning; 9.0.0->major 9, tooHigh empty
(proves quoting KEEPS numeric comparison: string cmp would give '9'>'10'=true); 10.0.0->empty;
11.0.0-beta->major 11, tooHigh=true. MY ADDITION: NO CI check reaches the fixed branch (needs a
ReferencePath lacking NuGetPackageVersion, which ResolvePackageAssets never emits) => green CI
only proves the happy path. Commented on both #13 and PR #12; rewrote summary #7.
Only re-engage if a human replies.

## Run log (short)
- **2026-10-31 12:36 UTC — QUIET (latest run).** State byte-identical to 2026-10-31 00:46.
  Issues 1,2,3,4,5,7,8,10,11,13 (no #14+); PRs still only 6,9,12 (open drafts; updated_at frozen
  at 2026-09-30T13:02:39 / 2026-10-14T12:53:13 / 2026-10-28T12:49:49 = all mine). #7 upd
  2026-10-28T12:49:50 = mine, 0 comments / 0 ticked boxes. Only #5 moved (2026-10-31T12:26:47).
  14 labels re-listed, AI-thinks-* still absent => Tasks 1-3 zero input (also oldest issue
  2026-08-29, past the 2024 cutoff). noop; #7 NOT updated per Task FINAL rule 5;
  c/woc/rtc=0, lr=2026-10-31.
  REMINDER: next run is almost certainly 2026-11 -> CREATE `[Repo Assist] Monthly Activity 2026-11`
  and set `ms` to it, but ONLY if that run has activity to report. Leave #7 open for the maintainer.

- **2026-10-31 00:46 UTC — QUIET.** State byte-identical to 2026-10-30 12:42.
  Issues 1,2,3,4,5,7,8,10,11,13 (no #14+); PRs still only 6,9,12 (open drafts, no new comments —
  checked PR #12 comments directly: only my own 2026-10-28). #7 has **0 comments / 0 ticked boxes**,
  updated_at 2026-10-28T12:49:50 = mine. #10 still only my comment. 14 labels, AI-thinks-* absent.
  Only #5 moved (2026-10-31T00:42:15). noop; #7 NOT updated; c/woc/rtc=0, lr=2026-10-31.
  NOTE: next run is likely 2026-11 -> must CREATE `[Repo Assist] Monthly Activity 2026-11` and
  set `ms` to it (only if there is activity to report); leave #7 open for the maintainer to close.

- **2026-10-30 12:42 UTC — QUIET.** State byte-identical to the 2026-10-30 00:56 run.
  Issues #1,#2,#3,#4,#5,#7,#8,#10,#11,#13 — no #14+. PRs still only #6,#9,#12, open drafts,
  updated_at frozen at my own comments. #7 upd 2026-10-28T12:49:50 = mine, 0 comments /
  0 ticked boxes. Only #5 moved (2026-10-30T12:34:38, machine noise). 14 labels, AI-thinks-*
  still absent => Tasks 2-3 zero input; oldest issue 2026-08-29 => Task 1 zero input.
  noop; #7 NOT updated per Task FINAL rule 5; cursors c/woc/rtc=0, lr=2026-10-30.

- **2026-10-29 00:55 / 12:42 and 2026-10-30 00:56 UTC — QUIET.** Byte-identical state each time;
  only #5 ticks. See the entry above for the full inventory.

- **2026-10-02 .. 2026-10-29 (50+ runs, twice daily): all noop except the four below.**
  ZERO human comments anywhere, ever. Every run re-lists the same issues/PRs with frozen stamps;
  only #5 ticks. 14 labels, AI-thinks-* always absent; no pre-2024 issues (oldest 2026-08-29).
  - 2026-10-14 12:49: new issue #8 + PR #9 (msbuild-quality) — duplicates of #4/#6. Commented on
    both; first update to summary #7.
  - 2026-10-20 01:08: NEW HUMAN ISSUE #10 (mab2500). Verified + commented; updated #7.
    Needs-Triage label added (14 total).
  - 2026-10-21 12:51: NEW ISSUE #11 (msbuild-quality). Verified all 3 findings, added 2 of my own
    + 1 correction. Commented; rewrote #7. #11 deduped correctly vs #4/#8.
  - 2026-10-28 12:41: NEW issue #13 + NEW PR #12 (msbuild-quality). Commented on both, rewrote #7.

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
