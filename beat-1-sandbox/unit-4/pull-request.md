# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/165

**Branch**

docs/73-add-openrouter-key-to-env-example

## Eval iterations

**Run history**

Run 1 (full run, original rubric): 13/20 agreed. All seven clear-accept packages (pkg-02, 05, 08, 11, 13, 16, 19) were rejected, so the clear-accept category scored 0/7 and the category floor was unmet.
Run 2: 7/7
Final run : 20/20

**Package analysis**

pkg-08 (jesseduffield/lazygit#5883, pressing `s` with only untracked files). The gold label is accept. In my first run my rubric said reject, failing "Repro before and after shown" and "Repo checks run and reported". In my last run my rubric said accept.

The first rubric read the package that way because it asked for a command and its printed output for every step and every repo check. pkg-08 is a terminal UI with nothing to print. Its evidence is the keys pressed and the text seen: `s -> "Stash name" prompt -> Enter -> nothing (stash list empty)` before, and `s -> toast "No tracked changes to stash..." -> no prompt; untracked.txt still listed` after. Its checks are reported by name with their outcome (`go test ./...` passes, `go generate ./...` produces no cheatsheet diff, "code formatted") but with no pasted output. The current rubric accepts the keys and the text seen as the observation for an interactive tool, and accepts a check reported by name with its outcome.

**Check rationale**

The check as it reads now in `tools/pr-precheck/rubric.md`:

> | Repro before and after shown | The test evidence (and the description's Testing section), read against the plan's test plan and the repro evidence's commands and outputs. | The issue's own repro is re-run, and the evidence shows both the before and the after: the command or steps run, the symptom seen before (the exact message, value, or behavior the repro evidence reports), and what was seen after, which matches the result the plan said to expect. The observation can be pasted output, a transcript with the result written beside each command (including in a comment), or, for an interactive tool with nothing to print, the keys pressed and the text or behavior seen, with any message quoted. The plan's other test steps (controls, success paths, unchanged behavior) pass if the evidence states their result in a sentence or a test in the diff covers them. Fail if the issue's repro is not re-run, if the before or the after is only a bare claim ("fixed", "works now", "passes") with no observed symptom or result, if the after differs from the plan's expected result with no explanation, if a secondary step is absent with no disclosure, or if the evidence contradicts a claim. A step disclosed as not run, with the reason, passes unless the description claims it passed. | required |

It reads this way because my first version failed all seven clear-accept packages on this check. It demanded a command and printed output for every step in the plan's test plan, and it said a note after `#` describing what a command printed was not output. Several accepted packages write the result beside the command in a comment (pkg-11, pkg-13), one is an interactive tool (pkg-08), and others cover a control or success path with a sentence or a test in the diff (pkg-02, pkg-05). I rewrote it so that the issue's own repro must show a concrete observed before and after, in any of those forms, and the other steps only have to state a result or be covered by a test. I kept the fail conditions for a repro that was not re-run, a bare claim like "fixed", and a contradicted claim, because those are what a not-tested package looks like.

**Trade-offs**

This check now accepts a stated result without raw output for the plan's secondary steps, so a PR that writes only "control unchanged" passes here even though nothing proves it. It also accepts a result written in a comment beside a command. What it gives up is strictness on how the proof is shown; it still fails a repro that was never re-run or is only claimed.
