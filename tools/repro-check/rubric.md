# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | repro report environment section | Explicitly specifies OS/platform, runtime/tool version, and tested version or commit SHA (fails if environment details are missing or completely unstated) | required |
| repro-steps-complete | repro report steps or preparation section | Contains runnable commands, script, test invocation, or clear description of inputs enabling an engineer to run the reproduction without ambiguity | required |
| reproduces-target-bug | repro report observed output vs issue description | Accurately tests the issue: if confirming reproduction, artifacts show the specific bug or failure described; if reporting an evidenced non-reproduction (cannot-reproduce), artifacts show the commands executed and the absence of the failure | required |
| honest-outcome | repro report artifacts vs stated conclusion | The stated conclusion faithfully matches what the artifacts show. Confirms reproduction only if the target bug occurred. Honestly states cannot-reproduce if the bug did not occur under the tested conditions (reject if passing/unrelated errors are claimed as confirming the bug) | required |
| claim-no-overpromise | candidate claim comment | If a claim comment is present, it explicitly states an intent to investigate, reproduce, or diagnose the issue. It does NOT promise a guaranteed fix, PR, or arbitrary deadline/timeline, and does NOT consist of an unsupported '+1' claim | required |
| policy-and-disclosure | repo-facts contribution policy and package text | Package complies with repository contribution policy (If repo policy requires AI disclosure, disclosure is present. If AI is prohibited, no AI usage is claimed. Silence passes) | required |
| clean-artifacts | repro report output artifacts | If output artifacts are present, excerpts are scoped to relevant errors or logs rather than dumping unformatted, extraneous terminal buffer | preferred |

## Verdict rule

Accept if and only if every `required` check passes. If any `required` check evaluates to fail or unclear (except for checks explicitly skipped during a claim-only draft per SKILL.md), the verdict is reject. Preferred checks never alter the accept/reject verdict, they only evaluate formatting quality.
