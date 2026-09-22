# Rubric: is this a good first issue?


## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-alive | repo-facts default branch commits | At least 1 commit to default branch within the last 90 days | required |
| policy-allows-ai | repo-facts contribution policy | Repo contribution policy does NOT state that AI-generated code, documentation, or contributions are forbidden/banned/not accepted | required |
| maintainer-engaged | repo-facts default branch commits or maintainer sample | At least 1 commit to default branch by maintainers/collaborators within the last 30 days OR maintainer comment on an issue within 90 days | required |
| no-open-prs | repo-facts this issue linked PRs | No linked PRs with status `(open)` | required |
| unclaimed | issue assignees and comment thread | Issue has no assigned user AND no comment within the last 14 days by a contributor stating they are actively working on it | required |
| no-blocking-labels | issue labels | Issue does NOT have labels matching `wontfix`, `blocked`, `needs-design`, `needs-discussion`, `upstream`, or `duplicate` (excluding `duplicate::primary`) | required |
| vetted-or-actionable | issue author, labels, checklist, and body | If opened by a bot or non-member without triaged labels, it must have maintainer approval or checked prerequisites; AND must describe a concrete bug, doc update, or agreed feature (not speculative features with assets TBD or multi-month rewrites) | required |
| uncontended-discussion | comment thread count | Comment thread has fewer than 25 comments (reject issues with excessive contributor churn or long-term unresolved claim disputes) | required |
| good-first-label | issue labels | Issue has a label like `good first issue`, `beginner-friendly`, `help wanted`, or `easy` | preferred |

## Verdict rule

Accept if and only if every `required` check passes. If any `required` check evaluates to fail or unclear, the verdict is reject. Preferred checks do not alter the accept/reject verdict; they only rank accepted candidates.
