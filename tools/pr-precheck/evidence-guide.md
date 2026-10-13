# Evidence guide: where evidence lives in a PR package

A PR package (an eval "package" file like `calib-01.md`, or live drafts plus your branch and the GitHub issue) holds the plan context, a repo-facts block, the candidate PR (diff, commits, description), and the test evidence. Find each part by its content, since labels can vary. In eval mode, use only what is in the package.

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- *Eval bundle:* the plan-context block holds the files the plan will change (listed after "Files:"), the in-scope and not-in-scope statements, the approach, and any deviation notes. The candidate PR's diff section shows what changed. The description's Summary, Changes, and checklist hold the claims about it.
- *Live:* `plan.md` in the top folder of the clone (read its `## Deviations` section last), and the diff from `git diff main...HEAD`. The description is in `pr_draft.md`, under the title line.

**What good looks like.** Every changed file in the diff is named by the plan (by path, by folder, or by kind: "a changelog entry" names the changelog file, "a test" names the test file) or by a deviation note. Every approach step shows up in the diff or in a note that says it was dropped. Each description claim about the code has a diff line that makes it true (claims about what a command printed belong under Test evidence), and a claim that something was not added is true when nothing in the diff adds it. Silent drift looks like a file or hunk the plan never mentioned, a planned change missing with no note, a ticked "tests added" box for a case no test covers, or "exactly the plan, nothing more" next to an unplanned hunk. An honest deviation is one written in the deviation notes (or, in the description, naming the file and the reason) so the mismatch is tied back to the plan.

## Test evidence (harness category: not-tested)

**Where it lives.**
- *Eval bundle:* the candidate PR's test-evidence section (and the description's Testing section), read against the plan context's test plan and the repro evidence's commands and outputs.
- *Live:* `test_evidence.md` next to `plan.md`, and the Testing section of `pr_draft.md`. The repro comment on the issue holds the "before" output. The repo's own checks are the ones `.github/PULL_REQUEST_TEMPLATE.md` lists (here: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`).

**What good looks like.** The issue's own repro is shown before and after, each with a concrete observed result: pasted output, a result written beside the command (a comment is fine), or, for an interactive tool, the keys pressed and the text or behavior seen. The after matches what the plan said to expect. The plan's other steps (controls, success paths) have their result stated, or a test in the diff covers them. Each of the repo's listed checks is reported by name with its outcome ("cargo test passes (312 tests)", "fmt and clippy clean", "no cheatsheet diff"); raw output is not required. A non-result or a failing check reported with its reason is evidence too. These are not evidence: "fixed" or "works now" with no observed symptom or result, "tests pass" or "tested locally" that does not say which check, an automated test that would pass with or without the fix, or a placeholder such as "[paste output here]".

## Diff quality (harness category: unreviewable)

**Where it lives.**
- *Eval bundle:* the candidate PR's diff section and commit list.
- *Live:* `git diff main...HEAD` (all hunks), `git diff main...HEAD --stat` (the file list), and `git log main..HEAD --oneline` (the commits).

**What good looks like.** The fix is visible and nothing unrelated rides along: each hunk serves the change the plan names. Debris tells are debug output left in (print or log lines), commented-out code, TODO or scratch notes, whitespace or formatting changes on lines the fix does not touch, stray or generated files (including `plan.md`, `pr_draft.md`, `test_evidence.md`, which are never committed), and drive-by edits, renames, or refactors the plan does not name. A big hunk the plan names is not debris. In live mode `--stat` should list only the files the fix changes.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval bundle:* the repo-facts block (the PR template's sections, the contributing instructions, the AI-use policy, branch and title formats, and maintainer comments in the thread), read against the candidate PR's title, description, and commits.
- *Live:* `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the Path Review repo, the issue thread on GitHub, the branch name (`git branch --show-current`), and the title and description in `pr_draft.md`.

**What good looks like.** Every section the template asks for is filled with content about this change, not the template's placeholder, and any "N/A" gives a reason. "Closes #" names the plan's issue. The AI-use disclosure says in plain words how AI was used on this change. The branch name has a type prefix, the issue number, and a short description (like `docs/73-add-openrouter-key-to-env-example`). The other asks in CONTRIBUTING are met, and a maintainer's direction on the thread is followed or answered. Boilerplate looks like a description that would read the same on any PR, a section left as the template's own text, or a stated ask visibly skipped. Whether the description's claims match the diff is plan fidelity, above, not this heading.
