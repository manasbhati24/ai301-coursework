# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56)

**Verdict output**

#56 — Structural chunker silently drops documents with no headings
- repo-alive ✅ · policy-allows-ai ✅ · maintainer-engaged ✅
- no-open-prs ✅ timeline shows only 4 labeled events, no cross-references; PR search for #56 returns 0
- unclaimed ✅ assignees: [], comments: 0
- no-blocking-labels ✅ bug, good first issue, ingestion, tier-1
- vetted-or-actionable ✅ opened by a COLLABORATOR, concrete repro and a named failing test
- uncontended-discussion ✅ 0 comments
- good-first-label ✅ (preferred) good first issue

```json
[
  {
    "item": "[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56)",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16T21:42:18Z, 5 days before today (2026-09-21)"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md (189 lines) and .github/PULL_REQUEST_TEMPLATE.md contain no AI/generated/disclosure terms; no AI_POLICY.md — silence passes"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Aburke225 (author_association COLLABORATOR) committed to main 2026-09-16, within 30 days"},
      {"name": "no-open-prs", "grade": "pass", "evidence": "Timeline shows only 4 'labeled' events, no cross-referenced PRs; PR search for '#56' returns total_count 0"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [] and comments: 0"},
      {"name": "no-blocking-labels", "grade": "pass", "evidence": "Labels: bug, good first issue, ingestion, tier-1 — none blocking"},
      {"name": "vetted-or-actionable", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225 with triaged labels; concrete bug with runnable repro and named failing test test_document_with_no_headings"},
      {"name": "uncontended-discussion", "grade": "pass", "evidence": "comments: 0, far below the 25 threshold"},
      {"name": "good-first-label", "grade": "pass", "evidence": "Carries the 'good first issue' label"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Run 1 (full eval): agreement: 13/20 scored items (below the bar; category floor unmet: no match in policy)
- Run 2 (partial tuning via --only): agreement: 4/4 scored items on canary set (issue-01, issue-09, issue-12, issue-18)
- Run 3 (partial tuning via --only): agreement: 1/1 scored items on issue-19
- Run 4 (full eval): agreement: 17/20 scored items (below the bar)
- Run 5 (partial tuning via --only): agreement: 3/3 scored items on canary set (issue-14, issue-15, issue-20)
- Run 6 (full eval confirmation): agreement: 19/20 scored items

**Issue analysis**

Issue ID: `issue-12` (bookwyrm-social/bookwyrm#1133)

Gold label: `reject`

Rubric decision: `reject` (initially graded `accept` in Run 1, corrected in subsequent runs)

Reasoning: In Run 1, the rubric evaluated `issue-12` as accept because the issue body had clear enhancement requests and lacked assignees or blocking labels. However, inspecting the repository facts revealed:

contribution policy (CONTRIBUTING.md -> docs.joinbookwyrm.com/contributing.html, section "Generative AI"): "Meaningful human interaction is the whole point of BookWyrm. We do not accept AI-generated code or documentation."

Because coursework workflows use AI tooling and skills, contributing to a project with an affirmative prohibition against AI-generated code or documentation violates project policy. Adding the `policy-allows-ai` check caused the rubric to evaluate this check as fail, converting the verdict to `reject` and aligning with the gold label.

**Check rationale**

| uncontended-discussion | comment thread count | Comment thread has fewer than 25 comments (reject issues with excessive contributor churn or long-term unresolved claim disputes) | required |

Reasoning: Many older issues appear open because they have no assignee and carry a `good first issue` label, but their comment threads reveal high churn, conflicting PR attempts, or abandoned discussions. As seen in `issue-15` (zulip/zulip#19589), 97 comments recorded multiple contributors repeatedly claiming the issue via `@zulipbot claim` only to be unassigned after 14 days of inactivity, alongside closed PRs and design debate. Setting an explicit threshold of `< 25 comments` prevents newcomers from stepping into contentious or stalled issues while comfortably accommodating normal maintainer clarifications (which typically remain under 10 comments).

**Trade-offs**

What the check gives up: The `< 25 comments` threshold in `uncontended-discussion` purposely sacrifices older, highly discussed issues that might still be technically viable if a contributor is willing to read through extensive historical context. Specifically, it directly changed the outcome of `issue-15` from an accidental `accept` to a correct `reject` (verified via `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --only issue-15`). I accept that this rule will false-reject the rare issue where 30+ comments represent enthusiastic debugging help rather than churn, in exchange for guaranteeing zero collision risk and low communication friction on accepted first issues.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

Answer all three:

1. The issue's fit to your interests and to the time available.
This issue (text chunking) aligns closely with my technical experience. We also have a reproduction script and an existing unit test.
2. What the verdict identified correctly, and what you weighed that the rubric could not.
The verdict identified (correctly) that the repository is active, unblocked by competing PRs, and has zero existing claim comments. The rubric could not really weigh into the simplicity of the fix (compared to other issues I had chosen).
3. The anticipated difficulty in claiming it.
Not a lot of difficulty. No assignees, comments, or PRs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
