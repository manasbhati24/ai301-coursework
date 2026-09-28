# Voice guide: how I talk upstream

## Who I am in threads

I am a novice here to learn more about the codebase in question, specifically by reproducing then investigatin its issues. Readers scan expect direct observations and facts from reproducible behavior, along with any status updates, without any overpromising.

## Rules I write by

### Rule: never promise delivery timelines or dates

Do not set arbitrary deadlines or time commitments.

- Wrong: "I'll have the PR ready by Tuesday afternoon."
- Right: "I am looking into reproducing this on main and will report back here."

### Rule: show the facts

Focus on providing command output, tracebacks, and any other behaviors to demonstrate what is happening, rather than using qualitative adjectives.

- Wrong: "It quickly crashed; its broken."
- Right: "Running `<CODE>` returned `<ERROR/OUTPUT>` on <INPUT>"

### Rule: promise an investigation rather than a fix

State what you will investigate and reproduce, but never promise that the issue would be solved/fixed.

- Wrong: "I will fix this bug by tomorrow and submit a pull request."
- Right: "I'd like to investigate this issue and will share a reproduction report with my findings."

### Rule: the environment

Always specify the environment details, incluing OS, package versions, commit branch, etc.

- Wrong: "I ran this on my mac."
- Right: "Reproduced on macOS 13.0, Python 3.7, commit `foobar`."

## Things I never post

- Any guarantees of a working fix before root-causing
- Timelines or delivery dates, espeically speculative ones
- Vague claims without reproduction artifacts
- Defensive replies to maintainer questions/feedback
