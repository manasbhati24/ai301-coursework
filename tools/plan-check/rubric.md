# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | Plan's stated cause read against issue description and repro evidence | Identifies a concrete defect or mechanism that directly accounts for the target bug described in the issue and demonstrated by the repro artifacts; fails if it contradicts repro measurements, solves a different bug, or blames an unrelated component | required |
| scope-bounded | Plan's scope statement and files named | Defines a contained boundary: specifies the specific file(s), function(s), or component(s) to modify, either by explicitly naming what remains out of scope or by strictly confining edits to an isolated site; fails if open-ended, unbounded, or proposing system-wide rewrites | required |
| approach-actionable | Plan's changes or steps to execute | Specifies concrete code-level modifications or procedural steps that an engineer could begin executing immediately; fails if hand-wavy, exploratory ("poke around", "figure out"), or purely aspirational | required |
| test-plan-verifies | Plan's test plan read against repro steps | Defines an observable verification procedure (either a targeted automated test or a reproducible step-by-step check derived from the repro) that specifically validates the target bug is fixed; fails if it only runs generic test suites without checking the bug, or lacks observable pass criteria | required |
| thread-and-convention | Plan comment and approach read against thread highlights and repo facts (CONTRIBUTING.md, AI policy) | Respects maintainer guidance, thread consensus, and repository contribution rules (including AI policies, PR scope cautions, or specific issue instructions); fails if it ignores maintainer warnings or violates stated repository policy | required |
| no-overpromising | Plan and comment language regarding timelines, fixes, and certainty | Accurately frames the work as an investigation or proposed approach without guaranteeing fixes or committing to rigid deadlines; preferred check that notes tone and risk realism | preferred |

## Verdict rule

Accept if every required check passes. If any required check receives `fail` or `unclear`, the verdict is `reject`. The preferred check flags feedback but never alters the binary verdict on its own.
