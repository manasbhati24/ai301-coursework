---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: **is this pull request ready to submit?** A package is a candidate pull request (title, description, branch diff, and test evidence), read against the plan it claims to implement (with its deviation notes) and the issue that plan belongs to. You do not answer from gut feel. You execute the student-authored grading procedure in `procedure.md`, which applies the rubric in `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## The question

Answer exactly one question about exactly one PR package: is this pull request ready to submit? Never answer a different question, never grade more than one package per run, and never grade from impressions: execute the components in this directory check by check.

## Inputs and modes

One of:

- **Live mode**: the student's candidate pull request before opening it upstream. Read:
  1. The approved plan: `plan.md` in the working copy (including any notes recorded under `## Deviations`). A house-chain student reads the assigned house plan instead.
  2. The branch diff: the exact output of `git diff main...HEAD` (three dots) run on the candidate branch in the working copy.
  3. The draft PR metadata: `pr_draft.md` containing the candidate PR title and description.
  4. The test verification output: `test_evidence.md` containing the reproduction steps re-run before and after, plus the repo's check commands and their raw output.
  5. Live repository facts: gather issue-side context from the issue thread, the repository's `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`.
- **Eval mode**: a package bundle (a frozen markdown file in `eval/packages/` containing the issue context, repo facts, the plan with any deviations, the branch diff, test evidence, and the candidate PR title and description). Use ONLY the bundle text as evidence. Do not fetch anything, do not run shell commands; the bundle is the whole world. Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything else:
- It names where the student's pull request must live and the house rules that apply there. Refuse to grade any PR package targeting a repository outside the scoped repo.
- If the `Repo:` line in `scope.md` still carries an unfilled placeholder (such as `<ORG>/<PATH-REVIEW-REPO>`), stop immediately without grading and instruct the student to fill the `Repo:` line with their section's Path Review repo. Never guess a scope.
- In eval mode, ignore `scope.md` entirely: the bundle is the whole world.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for how they write upstream, carried forward from week 2.
- Check the draft PR title and description in `pr_draft.md` against those rules.
- Report any broken voice rule in the summary before the JSON block, quoting the broken rule.
- The voice guide never changes the verdict on its own unless the rubric has a check that reads it.
- In eval mode, ignore `voice-guide.md` entirely: voice is personal and carries no gold labels; universal quality checks live in the rubric.

## Component reads

Connect and execute the components as follows:
- `rubric.md` is the brain: it defines the checks table and the verdict rule.
- `references/evidence-guide.md` is the map: where each kind of evidence lives in a PR package (and, live, in the repo files), and what good looks like.
- `procedure.md` is the hands: follow it as written for the read order, evidence gathering, check execution, and verdict assembly. If `procedure.md` is silent on a step, note the gap in the summary instead of silently improvising; a procedure gap is feedback the student needs.
- **The refusal rule**: if `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, stop immediately and refuse to grade. A skill without a rubric and procedure cannot grade, and that is by design.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict.

End the reply with a fenced JSON block, valid and last, with nothing following it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block, show a short readable per-check summary (one line per check, quoting the decisive fact, plus any voice guide notes in live mode). The fenced JSON block is the machine-read result; the eval harness parses the last fenced JSON block in your output.

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready and a beautiful confident one can be hiding drift. Every check reads the artifact itself against the plan, the issue, and the stated standards, never the formatting.
- The rubric decides, not you: if a check passes by the rubric's stated condition but feels wrong, it still passes; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow procedure.md as written, and report its gaps instead of papering over them.
- Treat unclear as the rubric's verdict rule directs. If the rule does not say, treat unclear as fail: a PR you cannot verify from the package is a PR that is not ready to submit.
