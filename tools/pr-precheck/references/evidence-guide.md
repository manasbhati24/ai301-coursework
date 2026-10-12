# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

### Where it lives
- **Eval package bundle**:
  - The plan boundary: in `## Plan context`, under `Plan:` (Scope, Files, and Approach sections) and any explicit deviation statements.
  - The actual implementation: in `### Diff` (modified file paths, hunk headers, and line diffs) and `### Commits`.
  - The fidelity claims: in `### Description` (summary explanation and changes list).
- **Live mode**:
  - The plan boundary: `plan.md` in your working copy, specifically `## Proposed Approach` (Scope, Implementation) and `## Deviations`.
  - The actual implementation: the raw output of `git diff main...HEAD`.
  - The fidelity claims: `pr_draft.md` (Summary and Changes sections).

### What good looks like
- Every file and function touched in the diff is explicitly listed in the plan's declared scope, or explicitly explained and justified in a `## Deviations` note.
- Every architectural step or bug fix promised in the plan is present in the diff (no silent omissions).
- The PR description accurately reflects the exact edits in the diff without claiming fixes, features, or behavioral shifts not present in the code.
- Silent drift occurs when the diff edits unannounced files, drops promised work without a deviation note, or when the description claims fidelity that the diff contradicts.

---

## Test evidence (harness category: not-tested)

### Where it lives
- **Eval package bundle**:
  - Expected verification scope: in `## Plan context`, under `Test plan:` and `Repro evidence:`.
  - Captured evidence: in `### Test evidence` under the Candidate PR block.
- **Live mode**:
  - Expected verification scope: `plan.md` (`Verification` / `Test plan`) and your Unit 2 reproduction steps.
  - Captured evidence: `test_evidence.md` (reproduction before and after, plus repo checks: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`).

### What good looks like
- Contains real, observable command executions with full terminal outputs showing both the failing "before" baseline and the passing "after" state for every distinct defect or failure mode promised in the test plan.
- Shows that repository-mandated test suites or checks ran and passed without unaddressed or hidden assertion failures.
- Not-tested occurs when `### Test evidence` says `(none provided)`, substitutes bare claims like "tests pass" without command outputs, omits verification for one of multiple promised failure modes, or shows failing assertions.

---

## Diff quality (harness category: unreviewable)

### Where it lives
- **Eval package bundle**:
  - In `### Diff` and `### Commits`.
- **Live mode**:
  - In `git diff main...HEAD` and `git log main..HEAD --oneline`.

### What good looks like
- The diff contains only the minimal, cohesive changes required to resolve the bug, with clear commit messages describing the changes.
- Zero debug leftovers (`print()`, `console.log()`, `debugger`), zero commented-out dead code, and zero accidental temporary test files.
- Free of unrelated churn, including drive-by formatting rewrites (e.g., reformatting unaffected functions), unrelated dependency/lockfile bumps, or arbitrary refactorings outside the bug site.
- Untracked course scratch files (`plan.md`, `comment.md`, `pr_draft.md`, `test_evidence.md`) are completely absent from the diff.

---

## Standards and comms (harness category: standards-wall)

### Where it lives
- **Eval package bundle**:
  - Stated repo requirements: in `## Repo facts`, specifically `pull requests:` (template requirements, checklists) and `contribution policy (CONTRIBUTING.md)` (including AI usage policies).
  - Candidate compliance: in `### Title`, `### Description`, and `### Commits`.
- **Live mode**:
  - Stated repo requirements: the repository's `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.
  - Candidate compliance: `pr_draft.md` (Title and Description) and commit history.

### What good looks like
- The PR description uses the repo's required template, completing every mandated section and checklist without leaving fields blank, deleting template sections, or submitting unresolved placeholders.
- The PR references or links the target issue according to repo convention (e.g., `Closes #<id>` or referencing `#<id>` in title/description).
- The author provides an honest disclosure of AI tool usage whenever mandated by repository policy or course guidelines.
- Standards-wall occurs when a PR ignores required template checklists, skips required AI disclosures, violates stated contribution rules, or ignores explicit maintainer direction in the issue thread.
