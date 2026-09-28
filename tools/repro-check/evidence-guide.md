# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle: the environment record inside the repro report (usually near the top), read against the issue context, which says what version and platform the bug targets. In live mode: the environment section of the student's repro report draft, read against the issue thread on GitHub.

**What good looks like.** The record names the operating system and an exact version of the software under test (a release number or a commit hash). Those match what the issue targets, or the report says how they differ. A moving label such as "latest" or "current master" with no commit is not a version.

## Steps

**Where it lives.** In an eval bundle: the steps section of the repro report. In live mode: the steps in the draft report, checked against the repo's own setup docs (README, CONTRIBUTING).

**What good looks like.** Someone with the repo can reach the trigger from the steps. The commands and inputs specific to this bug are written out, and any file or input the trigger needs is given or created by an earlier step. Routine setup that the repo's own docs cover (clone, install) need not be repeated. A step that is visibly wrong (typo, wrong syntax, a file used but never created) does not count as followable.

## Behavior shown

**Where it lives.** In an eval bundle: the artifacts in the repro report (output excerpts, logs, screenshots), read next to the symptom described in the issue context. In live mode: the pasted output in the draft, read next to the issue's original description and any error text it quotes.

**What good looks like.** The artifact shows the same underlying symptom the issue describes (same kind of error, same wrong result). Wording, formatting, and line numbers may differ. An adjacent behavior (a different failure, a syntax or setup failure, or output that runs fine when the issue says it should fail) does not count, even if it is dramatic. Only if the excerpt is truncated so the two cannot be compared at all is the evidence unclear.

## Honesty

**Where it lives.** The conclusion in the repro report (reproduced or cannot reproduce), read against its own artifacts and environment record. Also any statement of cause or fix.

**What good looks like.** The conclusion says exactly what the artifacts show, no more. "Reproduced" is backed by an artifact showing the issue's behavior. "Cannot reproduce" is also honest when it shows the environment, what was run, and what was observed instead. Claims about the cause or a fix that no artifact shows are overclaiming.

## Comms

**Where it lives.** In an eval bundle: the claim comment and the repro report, read against the repo-facts block (templates, contribution policy, AI-use rules) and the issue context. In live mode: the draft comments, read against the repo's CONTRIBUTING file, issue and PR templates, and any AI policy, plus the issue thread.

**What good looks like.** The claim comment names something particular to this issue (its symptom or component), says what the author will do next (investigate and report back), and promises no fix and no date. Both comments follow the repo's stated templates. If the repo requires disclosure of AI assistance, the comments say so plainly. If no such rule exists, nothing more is needed. Boilerplate that could be posted on any issue is not specific.
