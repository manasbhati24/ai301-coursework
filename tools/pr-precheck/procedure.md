# Procedure: how this tool grades a PR package

## Read order

Execute the reading process in this exact sequence to support rigorous side-by-side comparison:

1. **Read the Issue & Repo Facts**: Identify the issue number, core bug report, expected vs. actual behavior, and repository contribution policies (including PR template asks, checklists, and AI disclosure requirements).
2. **Read the Candidate Plan**: Extract the declared diagnosis, in-scope files/functions, explicit out-of-scope declarations, architectural approach, and any notes recorded under `## Deviations`. Note every specific test scenario or failure mode promised in the test plan.
3. **Read the Branch Diff (`git diff main...HEAD` or package diff)**: Record every modified, added, or deleted file, every changed function signature, and every code hunk. Do not read the PR description yet—ground your understanding in what the code actually does first.
4. **Read the Test Evidence**: Record all command executions, test suite invocations, before/after reproduction outputs, and test pass/fail metrics. Check whether every test case promised in the plan appears here.
5. **Read the PR Description & Title (`pr_draft.md` or package PR block)**: Extract the PR title, summary explanation, linked issue, enumerated changes, testing checkboxes, and notes for reviewers.

Reading the code diff before the PR description ensures the evaluator is not biased by the author's claims and can spot drift immediately.

## Evidence gathering

Gather evidence pairs systematically across the four core dimensions:

1. **Plan Fidelity (Silent Drift)**:
   - Compare the files and functions modified in the diff against the plan's declared Scope and Approach.
   - If the diff touches files outside the plan's scope, check whether an explicit entry under `## Deviations` documents and justifies the change.
   - Check if any functional change promised in the plan is absent from the diff, or if the diff introduces unannounced architectural refactoring.
2. **Test Evidence (Not Tested)**:
   - Compare the commands and outputs in the test evidence against the plan's Test Plan and the issue's reproduction steps.
   - Confirm presence of both a failing "before" baseline and a passing "after" run for the target defect.
   
   - Verify that repository-required quality checks (e.g., `make check`, `make test-unit`, or test suites) were executed without unexplained test failures.
3. **Diff Cleanliness (Unreviewable)**:
   - Inspect every diff hunk for leftover debug statements (e.g., `print()`, `console.log()`), commented-out dead code, temporary test files, unrelated dependency bumps, or formatting/whitespace churn.
   - Confirm that untracked plan or scratch files (`plan.md`, `comment.md`, `test_evidence.md`) are not staged in the diff.
4. **Repo Standards (Standards Wall)**:
   - Compare the PR description against the repository's `.github/PULL_REQUEST_TEMPLATE.md` or package checklist requirements. Verify no required sections or checkboxes were deleted or left blank.
   - Check whether the repository mandates an AI disclosure in PR notes, and verify that the author provided an honest disclosure.
   - Verify the issue link is formatted correctly (e.g., `Closes #<id>` or referencing the issue number).
5. **Description Accuracy**:
   - Compare every bullet in the PR description's "Changes" section against the diff hunks.
   - Verify that the description does not claim features, fixes, or refactors not actually present in the diff.

## Check execution

Evaluate checks in the order they appear in `rubric.md`:

1. `plan-fidelity`: Grade `pass` if all diff hunks fall strictly within the plan's scope or are explicitly documented under deviations; grade `fail` if silent scope creep, omitted work, or unannounced architectural changes are detected.
2. `test-evidence`: Grade `pass` if real, observable test outputs confirm the defect is fixed, target bug verification or test suite passes are shown, and unit suites passed; grade `fail` if test evidence is absent ("none provided"), omits promised test cases, or shows unresolved errors.
3. `diff-cleanliness`: Grade `pass` if the diff is minimal, focused, and free of debug debris or unrelated churn; grade `fail` if stray prints, commented code, unrelated dependency changes, or drive-by refactorings are present.
4. `repo-standards`: Grade `pass` if PR template sections, checklists, AI disclosures, and contribution guidelines are satisfied; grade `fail` if required sections are skipped or policies violated.
5. `description-accuracy`: Grade `pass` if the description faithfully reflects the diff; grade `fail` if it contradicts the diff or makes unsupported claims.
6. `tone-realism` (preferred): Grade `pass` if tone is objective and factual; grade `fail` if bloated with unsupported superlatives or defensive justifications.

When evidence for a required check is missing from the package, grade that check as `fail` immediately with an evidence line noting the missing artifact.

## Verdict assembly

1. Apply the verdict rule from `rubric.md`:
   - If every required check receives `pass`, the final verdict is `accept`.
   - If any required check receives `fail` or `unclear`, the final verdict is `reject`.
2. Deciding check selection:
   - When one or more required checks receive `fail` or `unclear`, the deciding check quoted in summary reporting is the first failing required check in rubric table order.
3. Format the output:
   - Provide a concise narrative summary reviewing each check with its deciding evidence quote.
   - In live mode, report any voice rule warnings from `voice-guide.md` (which do not affect the verdict).
   - Conclude with the required JSON block as the final output. Set `item` to the issue URL or package bundle ID.
