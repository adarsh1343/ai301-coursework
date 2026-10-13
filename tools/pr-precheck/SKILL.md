---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this ready
to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against the plan it
claims to implement and the issue that plan belongs to. Grade one
package per run. Do not answer any other question: do not review the
code's design, do not suggest a better fix, and do not grade the plan
itself. The plan is the standard the PR is held to, not something you
grade. Do not answer from gut feel. Answer by executing the files in
this directory.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** grades the student's own submission before it goes out.
Read these inputs, all from the student's working copy unless noted:

- `plan.md`, including its `## Deviations` section. This is the
  standard the diff and the description are read against.
- The branch's diff: run `git diff main...HEAD` (three dots) from the
  top folder of the clone, and `git log main..HEAD --oneline` for the
  commit list. Only committed changes are part of the diff. If
  `git status` shows uncommitted changes to tracked files, say so in the
  summary and do not grade them.
- `pr_draft.md`: the PR title is its first line and the description is
  everything under it.
- `test_evidence.md`: the pasted output of the repro re-run and the
  repo's checks.
- The issue named in the student's request (its thread, read with `gh`,
  the GitHub API, or the web), for maintainer comments and for the
  student's repro comment.
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
  `docs/CONTRIBUTING.md`, for the repo's stated asks.

`plan.md`, `pr_draft.md`, and `test_evidence.md` are never committed, so
they will not appear in the diff. Read them from the working copy. If
one of them does appear in the diff, that is debris to report. A
house-chain student reads the house plan, the house repro pack, and the
house issue in place of the student's own, and the same checks grade the
same things.

In live mode, stop without grading and name what is missing if there is
no `plan.md`, no `pr_draft.md`, or if `git diff main...HEAD` is empty.
If only `test_evidence.md` is missing, grade anyway: the test checks
will fail for want of evidence, and that is the finding.

**Eval mode** grades a package bundle: a markdown file holding the plan
context, the repo-facts block, the candidate PR (diff, commits,
description), and the test evidence. The bundle is the whole world. Use
only its text as evidence. Fetch nothing, run nothing, and read no other
file. Eval mode always grades a complete package: run every check and
apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything else,
before the plan, the diff, or the drafts. It names the repository the
student's PR must live in and the house rules there. Follow those rules
when reading evidence. Refuse to grade a PR for any issue outside the
scoped repository, and say which repository is in scope.

If the `Repo:` line in `scope.md` is empty or still holds a bracketed
placeholder, stop without grading. Tell the student to fill the `Repo:`
line in `scope.md` with their section's Path Review repo and run the
tool again. Never guess a scope. In eval mode, ignore `scope.md`
entirely.

## The voice seam (live mode only)

In live mode, after reading `scope.md`, read `voice-guide.md`. It holds
the student's own rules for how they write upstream. Hold the outgoing
text, the PR title and the description, against every rule in it. For
each rule the text breaks, report it in the readable summary: quote the
rule's name and the offending words from the draft. The voice guide
never changes the verdict by itself. Only a rubric check can do that,
and only if that check reads the voice guide. If no check reads it,
report the broken rule and leave the verdict alone. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels.

## Component reads

Three files and a map decide how you grade. Use them together.

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. A check's pass condition is the only
  test for that check.
- `references/evidence-guide.md` is the map: for each kind of evidence a
  check names, it says where that evidence lives in an eval bundle and
  in a live working copy, and what good looks like there.
- `procedure.md` is the operating procedure. Execute it exactly as
  written, in order: the read order, how evidence is gathered, how each
  check is executed, and how the grades become a verdict.

When `procedure.md` is silent on something you need to decide, do not
invent a step. Carry on with the plainest reading of the rubric, and
list the gap in your summary under "Procedure gaps" so the student can
fix it.

If `rubric.md` has no filled-in checks, or `procedure.md` has no filled-in
steps (text inside comments does not count), refuse to grade. Say that
this tool cannot grade without both a rubric and a procedure, say which
one is empty, and stop. Do not make up checks at runtime.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject`
means hold. There is no third verdict, no "accept with reservations",
and no score. Put reservations in a check's evidence line.

Before the JSON block, write a short readable summary: one line per
check with its grade and evidence, then any voice-guide notes (live
mode), any procedure gaps, and, on a reject, the failed check that
matters most first. Then end your reply with the fenced JSON block
below. It must be valid, it must be the last fenced JSON block in your
reply, and nothing may follow it. The harness parses that block. Include
one entry in `checks` for every check in `rubric.md`, preferred checks
too.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Grade under these standing rules.

- **Evidence first.** Never grade a check without naming the fact or the
  short quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff, the claims, and
  the pasted output themselves. A short complete PR can be ready, and a
  long confident one can be hiding drift. Do not credit headings,
  length, or tone.
- **The rubric decides, not you.** If a check passes by its stated
  pass condition but feels wrong, it still passes. Mention the tension
  in the summary if you like. The fix belongs in the rubric, not in the
  run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps.
- **Honest shortfalls are not failures.** A PR that discloses what it
  left out or changed, in the plan's deviation notes or the
  description, is graded on what it says, not on being less than
  everything.
- **Treat `unclear` as the rubric's verdict rule directs.** If the rule
  is silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package is a PR that is not ready to submit.