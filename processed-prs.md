# Regression PR Shepherd — processed PRs

## dotnet/fsharp#20706 — "Add regression test: #6036, FCS namespace-module collision diagnostic"
- Last processed: 2026-10-08 (re-verified again, no change)
- Head sha seen: 5870e60c5512025affb8191503ff9d0f71343bd6
- Status: Category C (healthy). All 55 checks green, including
  `WindowsCompressedMetadata transparent_compiler_release` which previously failed.
- History: an earlier run (2026-10-06) posted a B3 comment claiming the bug still
  reproduced under the transparent compiler. That was subsequently addressed by a
  commit on the PR branch fixing `src/Compiler/Service/TransparentCompiler.fs`
  (capture project-finalization diagnostics) plus a release note. The PR therefore
  now contains a real product fix, not only test files — do NOT attempt to rebase,
  trim, or otherwise modify it. mergeable_state=blocked simply means it awaits
  maintainer review (abonie, T-Gro are requested reviewers).
- No review comments / review threads exist yet. Nothing to address.
- Next run: only act if new human review feedback or a CI failure appears after
  sha 5870e60.
