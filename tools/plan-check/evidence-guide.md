# Evidence guide: where evidence lives in a plan package

A plan package (an eval "package" file like `calib-01.md`, or live drafts plus the GitHub issue) holds five kinds of material: the issue context and thread highlights, a repo-facts block, a repro-evidence block, the candidate plan, and the candidate plan comment. Find each section by its content, since labels can vary. Use only what is in the package in eval mode.

## Diagnosis and grounding

**Where it lives.** The plan's stated cause is in the plan's diagnosis or "what causes this" section. The facts it must explain are in the repro-evidence block: the observed behavior, the commands and their output, and any files, lines, or settings the evidence names. Live: the stated cause is in the draft `plan.md`, and the repro evidence is the student's posted repro comment on the issue.

**What good looks like.** The stated cause points at the same files, lines, or settings the repro evidence shows, and quotes or cites that evidence. It explains every observed symptom and contradicts none. Bad: a cause that sounds plausible but names code the evidence never touched, or a cause that differs from the one the evidence points at. Bad: no stated cause at all, only a description of the symptom.

## Scope

**Where it lives.** The plan's in-scope statement (what will change), its not-in-scope line (what will not), and the list of files or areas it names. In the plan comment, the same boundaries should be repeated.

**What good looks like.** One bounded change: every named file or area ties back to the issue, and the not-in-scope line names a topic that was left alone on purpose ("will not change how config.py loads keys"). A drive-by rewrite looks like extra files, refactors, renames, or "while I'm here" fixes that the issue does not ask for, or like a vague area ("the config system") with no files named.

## Executability

**Where it lives.** The plan's files-to-touch list, its approach section, and any stated order of work.

**What good looks like.** A stranger could start right now: each file is named, each file has a described change, and the order is clear when it matters. Bad: steps that state a goal only ("make the docs consistent") or that depend on a decision the author has not made.

## Test plan

**Where it lives.** The plan's test plan section, read against the repro-evidence block's steps and artifacts (the commands run and the output seen before the fix).

**What good looks like.** It names the repro steps that will be re-run, and states what output or behavior will be seen after the fix, so the before and after can be compared. Manual or automated both count. A vague test plan says "test that it works" or adds a test that would pass with or without the fix.

## Honesty

**Where it lives.** The plan's risks and unknowns section, any "assumes" or "should" statements in the plan and comment, and (after a build) the Deviations section at the end of the plan.

**What good looks like.** What the evidence does not show is labeled as an unknown or risk ("I have not checked whether the Docker setup also reads this variable"). False confidence is a claim about code or behavior stated as fact with nothing in the repro evidence or repo-facts to support it, or a plan with an obvious risk and an empty risks section. A recorded deviation (what changed and why) is honest; a change that only appears in the diff is not.

## Comms

**Where it lives.** The plan comment, read against two other places: the thread highlights in the issue context (comments by a maintainer, collaborator, or repo owner) and the repo-facts block (stated templates, contribution asks, claim rules, and any AI-use disclosure requirement). Live: the issue thread on GitHub, and the repo's CONTRIBUTING file and issue and PR templates.

**What good looks like.** Thread-aware: when a maintainer has suggested a direction, set a constraint, or objected, the comment follows it or says why the plan differs. Convention-aware: the comment includes everything the repo asks for. Boilerplate looks like a comment that would read the same on any issue, with no mention of what was said in the thread. If the thread has no maintainer comments, or the repo states no asks, there is nothing to ignore, and the check passes.