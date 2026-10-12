# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Diff's changed files, functions, and logic read against the candidate plan's scope, approach, and explicit deviation notes | Every code change in the diff falls within the plan's declared scope or is honestly documented under deviations; fails if the diff silently introduces unmentioned functional changes, modifies unplanned files without deviation notes, or omits planned work without disclosure | required |
| test-evidence | Test evidence output read against the plan's test plan, reproduction steps, and repo test commands | Demonstrates that the fix was verified—either via observable before/after reproduction runs of the target bug, documented test scenarios, or passing test suite runs; fails if test evidence is absent ("none provided"), purely hand-wavy without checking the target defect, or shows unresolved failures | required |
| diff-cleanliness | Full diff hunks read across all modified, added, or deleted files | Diff contains only changes strictly necessary for the planned fix; fails if polluted with unrelated dependency bumps, cosmetic refactorings, drive-by formatting churn, debug logging, or commented-out dead code | required |
| repo-standards | PR title, description, and template sections read against repo facts, PR template requirements, and contribution policies | Complies with all repository guidelines, including filling out all required PR template sections/checklists, providing required AI usage disclosures where mandated, and linking the issue; fails if required template fields are deleted/blank or repository contribution rules are violated | required |
| description-accuracy | PR description summary and changes list read against the actual diff hunks | Accurately describes what the diff actually modifies without misrepresenting changes or claiming fixes/features that do not exist in the code; fails if the description asserts behavior or changes that directly contradict the diff | required |
| tone-realism | PR title, description, and notes for reviewers | Accurately frames work as a completed change addressing the specified issue without promotional fluff, defensive rationalizations, or unsubstantiated performance/architectural claims | preferred |

## Verdict rule

Accept if every required check (`plan-fidelity`, `test-evidence`, `diff-cleanliness`, `repo-standards`, `description-accuracy`) receives `pass`. If any required check receives `fail` or `unclear`, the verdict is `reject`. The preferred check (`tone-realism`) reports feedback in the summary but never alters the binary verdict on its own.
