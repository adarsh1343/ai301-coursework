# Procedure: how this skill grades a plan package

These steps grade a plan. They do not make one. Do exactly what each step says, in order.

## Read order

Read in this order, and write down the listed facts from each part before moving on. The order matters: the repro evidence and the thread are the standard the plan is judged against, so they are read before the plan, and the plan comment is read last so it is judged against everything already known.

1. Read the repro evidence block (live: the student's posted repro comment on the issue). Write down: the behavior observed, the commands and outputs, and every file, line, or setting the evidence names as involved in the cause.
2. Read the issue context and thread highlights (live: the issue thread). Write down every comment by a maintainer, collaborator, or repo owner, and any direction, constraint, objection, or claim ("I'm taking this") in it. If there are none, write "no maintainer comments".
3. Read the repo-facts block (live: the repo's CONTRIBUTING file, issue and PR templates, and README contribution section). Write down every ask made of a contributor: required fields, templates, AI-use disclosure, claim rules. If there are none, write "no stated asks".
4. Read the candidate plan. Write down: the stated cause, the in-scope statement, the not-in-scope line, the files or areas named, the approach and its order, the test plan, and the risks and unknowns.
5. Read the candidate plan comment. Write down whether it states the cause, change, and test plan, and whether it quotes repro evidence.

## Evidence gathering

Gather evidence for each check from the parts named here, using the facts already written down. Where to look within each part is in `references/evidence-guide.md`.

1. Diagnosis matches repro: put the plan's stated cause (step 4) next to the repro facts (step 1). Record whether the cause names the same files, lines, or settings, and note any repro fact the cause does not explain.
2. Fix targets the cause: put the plan's list of changes next to its stated cause. Record, for each change, whether it removes the cause or only changes the visible symptom.
3. Scope is bounded: record the files or areas named, the not-in-scope line (quote it), and any change that has no connection to the issue.
4. Executable by a stranger: record, for each file named, what the plan says to do in it, and whether the order of work is stated.
5. Test plan is observable: put the test plan next to the repro commands (step 1). Record which repro steps are re-run and the exact expected result after the fix (quote it).
6. Unknowns are stated honestly: record every claim in the plan or comment about code or behavior, and mark whether the repro evidence or repo-facts support it. Record the risks and unknowns the plan lists.
7. Thread-aligned: put each maintainer, collaborator, or owner comment (step 2) next to the plan and comment. Record whether each is followed, answered, or ignored.
8. Repo conventions followed: put each repo ask (step 3) next to the plan comment. Record whether each is satisfied.
9. Comment stands alone: record whether the cause, the change, the test plan, and quoted repro evidence can all be found in the comment itself.

## Check execution

1. Run the checks in the order of the rubric table, top to bottom.
2. Grade each check `pass`, `fail`, or `unclear`, using only the pass condition in the rubric and the evidence recorded for it. Every grade needs one line of evidence: a fact or a short quote.
3. If the evidence for a check is missing from the package, grade it `fail` when the plan was supposed to provide it (cause, scope, files, approach, test plan, risks). Grade it `pass` for the two automatic-pass cases: no maintainer comments in the thread, and no stated repo asks.
4. Grade `unclear` only when the evidence is present but the pass condition cannot be decided from it (for example, the plan names a cause and the repro evidence is too thin to tell if it matches). Say in the evidence line what is missing.
5. Do not re-read the whole package for each check. Use the facts recorded in the read steps. Go back to the source text only to copy an exact quote, or when a recorded fact is too vague to decide a check.
6. Do not let one check's grade change another's. A plan can fail Scope and still pass Diagnosis.
7. The plan's polish, length, and headings are not evidence. Grade what the plan says it will do.

## Verdict assembly

1. List the grades of all `required` checks. Ignore `preferred` checks for the verdict.
2. If every `required` check is `pass`, the verdict is `accept`.
3. If any `required` check is `fail`, the verdict is `reject`.
4. Treat every `unclear` on a `required` check as `fail`, so it also gives `reject`.
5. In the readable summary, name each failed or unclear required check and quote the evidence that decided it. If the verdict is `reject`, put the most important failed check first.
6. Write the JSON block last, with one entry per check (including `preferred`) and the verdict. Put nothing after it.