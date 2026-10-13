# Procedure: how this skill grades a PR package

These steps grade a PR. They do not write one, fix one, or review its design. Do exactly what each step says, in order.

## Read order

Read in this order, and write down the listed facts from each part before moving on. The order matters: the plan and the repo's asks are the standard the PR is judged against, so they are read before the diff, and the description is read after the diff so its claims are judged against what the diff really does.

1. Read the plan context (live: `plan.md`). Write down: the files it names, the in-scope statement, the not-in-scope line, each step of the approach (marking any that are conditional), the test plan's commands and expected results, and every entry under Deviations. If the Deviations section says "No deviations", write that.
2. Read the repro evidence and the issue thread (live: the student's repro comment and the issue's comments). Write down: the repro commands and the outputs they printed, and every comment by a maintainer, collaborator, or owner, with any direction, constraint, or objection in it. If there are none, write "no maintainer comments".
3. Read the repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the repo's AI-use policy). Write down: each section the template asks for, each check the repo says to run, the AI-disclosure ask, and each other ask (branch name, title or commit format, separate-PR rules, expected-failure markers). If there are none, write "no stated asks".
4. Read the diff and the commit list (live: `git diff main...HEAD` and `git log main..HEAD --oneline`). Write down: every changed file, what each hunk does in one phrase, and anything that looks like debris (debug output, commented-out code, scratch notes, whitespace-only changes, stray files).
5. Read the PR title and description (live: `pr_draft.md`). Write down: the title, each section and whether it holds real content, the issue reference, the AI-use disclosure, every sentence that claims something about the change, and every ticked checklist box.
6. Read the test evidence (live: `test_evidence.md`). Write down: each command shown, the output printed under it, and anything that is only a sentence, a comment after `#`, or a placeholder.

## Evidence gathering

Gather evidence for each check from the facts already written down. Where to look within each part is in `references/evidence-guide.md`.

1. Diff stays inside plan: put each changed file (step 4) next to the plan's files, scope, and deviation notes (step 1). Record, for each file, whether the plan names it by path, folder, or kind, or a deviation note or the description covers it. Then put each approach step next to the diff, and record whether it appears, was dropped with a note, or is missing with no note. For each changed file, record whether its hunks do what the plan says for it.
2. Description claims match diff: take each claim about the code and each ticked box (step 5) and find the diff line that makes it true. Record the line, or record "no supporting line". For each "not added" claim, record whether anything in the diff adds it. Set aside claims about what a command printed or how the program behaved: checks 3 and 4 grade those. Record any "only", "exactly", or "nothing beyond" claim and whether the diff contradicts it. A changed file the description does not mention is not a finding by itself.
3. Repro before and after shown: put each test plan step (step 1) next to the test evidence (step 6). Record the command shown, the output printed, whether a before is shown or quoted from the repro comment (step 2), and whether the output matches the expected result (quote it). For the issue's own repro, record whether the before and after each state a concrete observed result (pasted output, a result noted beside the command, or the text or behavior seen in an interactive tool), or only a bare claim. For the other test plan steps, record whether the result is stated or a test in the diff covers it. Record any placeholder and any step disclosed as not run.
4. Repo checks run and reported: put each check the repo lists (step 3) next to the test evidence and the description's Testing section. Record, for each, the check's name and its stated outcome, or the disclosure that it was not run. Record every claim that checks passed that does not say which check.
5. Diff is reviewable: record each debris item from step 4 with the file and the first offending line. Record whether each hunk serves the change the plan names. Record any of `plan.md`, `pr_draft.md`, `test_evidence.md` that appears in the diff.
6. Template and disclosure complete: put each template section (step 3) next to the description (step 5). Record whether it is present, whether it holds real content or the template's placeholder, and what any "N/A" says. Record the issue number after "Closes #" next to the plan's issue number. Record the AI-use disclosure, or that it is absent.
7. Repo rules and thread followed: put each other repo ask (step 3) next to the branch name, the title, the commit messages, and the diff. Put each maintainer comment (step 2) next to the description. Record each as met, answered, or ignored.
8. Title names the change: record the title and the file or behavior it names, next to the diff's changed files.

## Check execution

1. Run the checks in the order of the rubric table, top to bottom.
2. Grade each check `pass`, `fail`, or `unclear`, using only the pass condition in the rubric and the evidence recorded for it. Every grade needs one line of evidence: a fact or a short quote.
3. If the evidence a check needs is missing from the package, grade it `fail` when the PR was supposed to provide it (test output, a described change, a template section, the disclosure). Grade it `pass` for the automatic-pass cases: no repo checks listed and none claimed, and no other repo asks with no maintainer comments.
4. Grade `unclear` only when the evidence is present but the pass condition cannot be decided from it (for example, the diff is cut off in the middle of a hunk). Say in the evidence line what is missing.
5. Do not re-read the whole package for each check. Use the facts recorded in the read steps. Go back to the source text only to copy an exact quote, or when a recorded fact is too vague to decide a check.
6. Do not let one check's grade change another's. A PR can fail "Description claims match diff" and still pass "Diff stays inside plan".
7. Do not count a disclosure against the PR. If a deviation note or the description records what was left out or changed, grade the check on what the disclosure says.
8. The description's polish, length, and headings are not evidence. Grade what the diff does and what the output shows.

## Verdict assembly

1. List the grades of all `required` checks. Ignore `preferred` checks for the verdict.
2. If every `required` check is `pass`, the verdict is `accept`.
3. If any `required` check is `fail`, the verdict is `reject`.
4. Treat every `unclear` on a `required` check as `fail`, so it also gives `reject`.
5. In the readable summary, name each failed or unclear required check and quote the evidence that decided it. If the verdict is `reject`, put the most important failed check first.
6. Write the JSON block last, with one entry per check (including `preferred`) and the verdict. Put nothing after it.