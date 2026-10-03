# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

### Where it lives
- Eval mode: In the candidate plan under `Diagnosis`, `Cause`, or `Summary`, read directly against the package's `Repro evidence` section (and the original `Issue` description).
- Live mode: In `plan.md` under `## Diagnosis` (or `## Cause`), read directly against your posted repro comment from Unit 2 and the upstream issue description.

### What good looks like
- The plan identifies a specific, plausible code mechanism or defect that directly accounts for the symptoms and measurements demonstrated in the repro evidence.
- It is consistent with all reported controls and measurements.
- Fails if the stated cause contradicts observed measurements (e.g. blaming a pager when the repro shows syntax highlighting takes 25s without a pager, as in `calib-03`), targets a superficial symptom while ignoring the underlying cause, or invents an unsupported theory.

## Scope

### Where it lives
- Eval mode: In the candidate plan under `Scope`, `Boundary`, `Files to change`, or within `In scope` and `Out of scope` sub-clauses.
- Live mode: In `plan.md` under `## Scope` (specifically reading what is explicitly in scope vs. out of scope).

### What good looks like
- The plan explicitly defines the boundaries of the fix: it specifies the exact files, modules, or functions to be modified, AND explicitly states what adjacent features, behaviors, or files will remain untouched.
- Fails if the change is open-ended, proposes structural refactors beyond what the bug requires, or omits negative boundaries ("what we won't touch").

## Executability

### Where it lives
- Eval mode: In the candidate plan under `Approach`, `Changes`, `Implementation Steps`, or `Plan`.
- Live mode: In `plan.md` under `## Approach` or `## Implementation Steps`.

### What good looks like
- Concrete, actionable instructions that an external engineer could follow to implement the change without asking the author clarifying questions.
- Names specific APIs, callbacks, variables, conditions, or logic alterations.
- Fails if the steps rely on vague aspirations or deferred exploration (e.g. "poke around the code this weekend", "figure out how undo works", or "try tweaking some settings", as in `calib-02`).

## Test plan

### Where it lives
- Eval mode: In the candidate plan under `Test plan`, `Verification`, or `Validation`, read directly against the `Steps` and `Artifact` in the `Repro evidence`.
- Live mode: In `plan.md` under `## Test plan`, read against your Unit 2 reproduction steps.

### What good looks like
- Specifies an observable check that directly validates the target bug is resolved: either by re-running the exact repro steps and asserting the fixed outcome, or by adding a targeted unit/integration regression test.
- Fails if it merely specifies running the existing project test suite (e.g. `cargo test --workspace` or `pytest`) without adding or running a check that specifically targets the reported bug (as in `calib-04`), or if the expected post-fix outcome is unobservable.

## Honesty

### Where it lives
- Eval mode: In the candidate plan under `Risks`, `Unknowns`, or `Assumptions`, and within the candidate plan comment text.
- Live mode: In `plan.md` under `## Risks and Unknowns` (and post-build under `## Deviations`), as well as the text in `comment.md`.

### What good looks like
- Openly acknowledges technical uncertainties, trade-offs, potential regressions, or assumptions requiring confirmation during development.
- In the comment and plan, work is framed as a proposed fix or bounded attempt; it avoids guaranteeing success, promising bug-free PRs, or committing to arbitrary delivery dates.
- Post-build, `Deviations` clearly documents any divergence from the original plan, or explicitly notes "no deviations occurred" in the author's own words.

## Comms

### Where it lives
- Eval mode:
  - The `Candidate plan comment` block.
  - The package's `Thread highlights` (maintainer notes, contributor replies).
  - The `Repo facts` block under `contribution policy (CONTRIBUTING.md)` or AI policy notes.
- Live mode:
  - The candidate comment draft in `comment.md`.
  - The live GitHub issue thread comments.
  - The target repository's `CONTRIBUTING.md`, `README.md`, or AI policies.

### What good looks like
- The plan comment is concise, polite, and directly addresses the issue context.
- Respects explicit maintainer guidance or cautions found in the thread (e.g. warnings about review bandwidth, API stability, or PR scope).
- Strictly complies with the repository's AI policy: includes required disclosures if mandated (e.g., human-in-the-loop review disclosures as required by repos like ripgrep in `calib-04`), and refrains from prohibited automated spam. If policy is silent on AI, absence of disclosure passes.
