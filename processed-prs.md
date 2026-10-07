# Regression PR Shepherd — processed PRs

## dotnet/fsharp#20706 — "Add regression test: #6036, FCS namespace-module collision diagnostic"
- Branch: `regression-test/issue6036-31ada475ab22bc5e`
- 2026-10-06: Category B3. Test failed under `TEST_TRANSPARENT_COMPILER=1`
  (`WindowsCompressedMetadata transparent_compiler_release`). Commented, recommended close,
  `AI-thinks-issue-fixed` removed from #6036.
- 2026-10-07: Commit `5870e60` (Copilot coding agent) added a real product fix in
  `src/Compiler/Service/TransparentCompiler.fs` + release note. All 55 CI checks green.
  PR `mergeable_state: blocked` = awaiting required review from T-Gro/abonie.
  Posted a correction/status comment retracting the "close this" recommendation.
- **Do not touch again**: PR now modifies `src/`, which is out of shepherd scope.
  No further comments unless new human review feedback appears after 2026-10-07T17:00Z.
- Issue #6036 remains open and correctly un-labeled.
