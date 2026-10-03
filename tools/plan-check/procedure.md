# Procedure: how this skill grades a plan package

## Read order

1. In live mode, confirm the issue URL matches `scope.md`. Halt if out of scope.
2. Read the package context before reading any part of the candidate plan:
   - **Repo facts:** Note repository policies, contributing guidelines, AI rules, and issue templates.
   - **Issue description & Thread highlights:** Note the reported symptom, affected versions, and any maintainer warnings, cautions, or consensus.
   - **Repro evidence:** Note the concrete environment, execution steps, baseline controls, and specific observed artifacts or measurements (e.g., timings, error logs, return values).
3. Read the candidate plan's Diagnosis, Scope, Approach, and Test Plan.
4. Read the candidate plan comment.
*Why this order matters:* You cannot judge whether a diagnosis is grounded, a scope is bounded, or a test proves the fix without first establishing ground truth from the issue context and repro measurements. Reading the plan first primes the grader to accept unverified assumptions.

## Evidence gathering

For each check, gather the exact pair of facts required:

- **`diagnosis-grounded`**: Extract the candidate's stated cause or mechanism. Compare it against the issue description and repro evidence's observed artifacts and measurements.
- **`scope-bounded`**: Extract the specific files, modules, or functions named to change, and check for explicit negative boundaries ("Out of scope") or strict isolation to a single site.
- **`approach-actionable`**: Extract the proposed code-level edits, algorithmic changes, or implementation steps. Check whether they describe concrete operations or speculative exploration ("poke around", "figure out").
- **`test-plan-verifies`**: Extract the verification steps. Compare them against the repro steps: does the test execute the failing scenario, assert the specific fixed behavior, or add a targeted test? Or does it merely run an existing regression suite?
- **`thread-and-convention`**: Extract the plan comment and overall approach. Check them against the repo facts (AI disclosure, PR size warnings) and maintainer notes in the thread highlights.
- **`no-overpromising`**: Extract commitment statements, deadlines, and certainty claims from both the plan and the draft comment.

## Check execution

Grade checks in deterministic sequence:

1. **Evaluate `diagnosis-grounded`**:
   - `pass`: The mechanism directly explains the target bug and repro artifacts without contradicting any observed measurement or control.
   - `fail`: Contradicts repro evidence, targets an unrelated bug/symptom, or invents facts.
   - `unclear`: Diagnosis is completely absent or cannot be evaluated against the repro artifacts.
2. **Evaluate `scope-bounded`**:
   - `pass`: Identifies specific target files or functions AND bounds the blast radius.
   - `fail`: Proposes open-ended refactoring, touches unspecified components, or lacks any containment.
   - `unclear`: Scope is missing.
3. **Evaluate `approach-actionable`**:
   - `pass`: Steps specify concrete modifications a developer could execute without guessing implementation intent.
   - `fail`: Relies on vague or future investigation.
   - `unclear`: Implementation approach is missing.
4. **Evaluate `test-plan-verifies`**:
   - `pass`: Describes an observable check specifically asserting the target bug is resolved.
   - `fail`: Only runs generic test commands without exercising the bug, or lacks observable pass criteria.
   - `unclear`: Test plan is missing.
5. **Evaluate `thread-and-convention`**:
   - `pass`: Complies with repo AI policy, respects maintainer warnings on scope/bandwidth, and aligns with thread consensus.
   - `fail`: Violates repo policy (e.g., missing mandatory AI disclosure) or proceeds directly against explicit maintainer direction.
   - `unclear`: Relevant thread or policy context cannot be determined.
6. **Evaluate `no-overpromising`**:
   - `pass`: Frames work as an investigation or proposed solution with appropriate uncertainty.
   - `fail`: Promises guaranteed fixes, specific PR delivery dates, or rigid timelines.

For each check, record a single-line evidence summary citing the specific text or omission that determined the grade.

## Verdict assembly

1. Apply the binary verdict rule:
   - If ALL required checks receive `pass`, the final verdict is `accept`.
   - If ANY required check receives `fail` or `unclear`, the final verdict is `reject`.
   - The preferred check does not alter the verdict, but any failure must be reported in the review summary.
2. If the verdict is `reject`, identify the primary failing check and include a clear, actionable explanation of what evidence caused the rejection.
3. Emit the final evaluation in the required JSON format.
