# Contribution: Document the setup and running of test_validation

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Adarsh Kessani
**GitHub Username:** adarsh1343
**Issue:** https://github.com/scikit-hep/pyhf/issues/1250
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a search-rank-grade scout for open source issues (2026-11-04)**

I turned my Unit 1 issue-select skill into issue-scout: three `gh search issues` queries written from my fit profile (Python, shell, and CI work; docs and config fixes), ranking filters that drop assigned, stale, long-debate, and frontend issues and sort by fewest comments, and my rubric to grade the top five. My first queries returned 0 results because quoting "docker OR deploy OR ci" made gh search for one exact phrase, and my scans showed the rubric passing brand-new repos: ros-envs#3 (2 days old, one contributor) passed "Repo in use" on a single release, and whyfull#28 (also about 2 days old) passed through the closed-PR route, while Canvas-Web#290 failed "Maintainer alive" even though its maintainer was reviewing PRs. I removed the quotes, made a 30-day minimum repo age apply to both routes of "Repo in use", counted PR reviews as maintainer activity, and fixed my verdict rule, which said "five required checks" when the table had four.

**Unit 6: Installed the voice guard, extended my voice guide upstream, and wrote a CLAUDE.local.md for pyhf (2026-11-15)**

I added an Upstream section (claims, questions, pings) to my voice guide, hooked `check_voice.py` into Claude Code as a PreToolUse hook on Bash, and wrote a 43-line `CLAUDE.local.md` in my pyhf clone covering CONTRIBUTING rules, the layout of `tests/test_validation.py`, and the install that actually works, kept out of my diff with `.git/info/exclude`. The wiring test showed a gap: on Windows, Claude Code's first attempt at `gh issue comment --help` went through its PowerShell tool, which my `"matcher": "Bash"` hook never sees, so the guard only stopped it on the second try, through Bash. The guard then stopped my claim draft twice, once for writing "all 20 pass" without the output and once for a "because" explaining the fixture lookup, and it stopped my repro report for saying "reproduced" when Claude had run the commands, so I quoted the printed output in the claim and re-ran the repro steps myself. The memory file records two install surprises I'd otherwise relearn each session: `.[xmlio]` alone dies on an `iminuit` warning filter, and the conftest needs `jax` even to run one file.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

In unit 5 my scout searched GitHub with three `gh search issues` queries built from my profile (Python, shell and CI work, docs and config fixes), ranked the results, and graded the top candidates. I also graded a few issues directly. After this week's scope patch I re-graded the four issues I had graded directly: amlaban65/whyfull#28, scikit-hep/pyhf#1250, ai-dynamo/dynamo#13211 and project-blurple/Canvas-Web#290. Only pyhf#1250 was accepted. whyfull#28 had been my favorite (Python CI tooling with the exact files named), but in the ten days since my first scan someone opened PR #38 saying "Fixes #28", and the repo is still under my 30-day age minimum. dynamo#13211 already has an open PR, and Canvas-Web#290's maintainer hasn't merged anything since August. For pyhf#1250 the scout passed all five required checks: matthewfeickert committed on 2026-11-15 and v0.8.0 shipped the same day, nobody is assigned, and the only comment is the maintainer's. Its difficulty note was **ambitious**, because the issue says the next PR should "either revise all of it or include some sort of walkthrough", and those are very different jobs. So my claim asks that question first.

It fits me because it's Python and pytest, it's a docs-and-tests task (squarely in my "fixing docs mismatches, small well-tested fixes" profile), and pyhf is a mature, actively maintained scientific library, which is the kind of project I want to learn to work in. I hope to learn how a large parametrized pytest suite is put together and how to write documentation that a maintainer actually wants to merge. Honest read of the scout: its verdicts held up on the facts. Every rejection was real (an open PR, a dead maintainer), and the re-grade caught whyfull#28 being claimed since last week, which I would have missed. But it has no check for how stale the issue itself is: pyhf#1250 was opened in January 2021 and untouched since, and the scout passed it on repo-level activity alone. I'd add an issue-age check, or at least a flag, and in the meantime my claim asks whether a walkthrough is still wanted.

Claim comment: https://github.com/scikit-hep/pyhf/issues/1250#issuecomment-6924195211

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I re-ran `claude "issue-scout: grade ..."` on the four issues I graded directly last week, after patching the rubric.
- What it produced: a read-out accepting only pyhf#1250 (all five required checks pass, difficulty note "ambitious: the issue says to 'either revise all of it or include some sort of walkthrough'; ask which first") and rejecting whyfull#28 (open PR #38 "Fixes #28", repo 13 days old), dynamo#13211 (open PR #15283 and an unanswered claim) and Canvas-Web#290 (no maintainer activity since 2026-08-08).
- Difficulties: my unit-1 "Scope fits a newcomer" check only passed issues with a beginner label or a single named file, which rejects most real issues that have an open edge. The scout also has no check for issue staleness, so a nearly 6-year-old issue passed on repo activity alone.
- What changed: I applied the scope patch, replacing "Scope fits a newcomer" with the required "scope-feasible" check (which only rejects umbrella or checklist issues) and adding the difficulty note, then logged it in `scan-notes.md` on 2026-11-15. I'd still add an issue-age flag next.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 Home (10.0.26200), Git Bash
- Python 3.14.7 in a fresh venv (`python -m venv .venv`)
- pyhf at `main` commit `4bbcb86f89a4c2c8962b2c442fb8fb62a14c9f76` (2026-11-15), cloned from my fork `adarsh1343/pyhf`; installed version reports `0.8.1.dev1`
- Install: `python -m pip install --editable ".[all]" --group test` (pytest 9.2.0, numpy 2.5.5, scipy 1.18.1, iminuit 2.33.0, jax 0.12.0.1, uproot 5.7.7)

Challenges: I first installed only `.[xmlio]` with the `test` group. pytest stopped before collecting anything with `PytestConfigWarning: Failed to import filter module 'iminuit'`, because the repo's pytest config has a warning filter that names `iminuit`. After I added `minuit`, collection stopped with `ImportBackendError: ('There was a problem importing JAX. The jax backend cannot be used.', ...)`, because `tests/conftest.py` sets up every backend. Installing `.[all]` fixed both. The repo's pytest config also turns on `--showlocals`, which makes tracebacks hundreds of lines long, so I used `--no-showlocals` for the failure excerpt.

**Steps to Reproduce**

1. Clone pyhf, create a venv, and install as above.
2. `wc -l tests/test_validation.py` and `grep -rln "test_validation" docs README.rst CONTRIBUTING.md`. The first prints `1024 tests/test_validation.py`; the grep prints nothing, so no walkthrough exists in the docs.
3. `python -m pytest tests/test_validation.py -q` prints `20 passed in 31.06s`. Nothing is failing today; the issue is about what happens when something does.
4. `python -m pytest "tests/test_validation.py::test_validation[2bin_2channel_couplednorm_mu1]" --setup-plan -q` prints `fixtures used: request, reset_backend, reset_events, setup, setup_cuda_environment`. The fixtures that define this configuration (`source_2bin_2channel_couplednorm` line 376, `spec_...` line 384, `expected_result_...` line 443) aren't listed, because `setup` fetches them by string at lines 752-754 with `request.getfixturevalue(f"source_{_name}")`.
5. Change the expected value on line 451 (`0.1361301880753241` to `0.1461301880753241`), run that test ID with `-q --tb=short --no-showlocals`, then revert with `git checkout -- tests/test_validation.py`.
6. Observed: the failure traceback names only `tests\test_validation.py:795: in test_validation` and `tests\test_validation.py:658: in validate_hypotest`. Nothing points at line 451 or `expected_result_2bin_2channel_couplednorm`, so you have to read `setup`, work out the f-string, and search the file, which is the "hunting down" the issue describes.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the failure in step 5 was induced on purpose and reverted; `git status --short` printed nothing afterward)
- **Screenshots/logs:** output of step 5, with only the captured `INFO`/`WARNING` log lines removed:

```text
_______________ test_validation[2bin_2channel_couplednorm_mu1] ________________
tests\test_validation.py:795: in test_validation
    validate_hypotest(
tests\test_validation.py:658: in validate_hypotest
    assert result == pytest.approx(expected, rel=tolerance), result
E   AssertionError: array(0.13613029)
E   assert array(0.13613029) == 0.1461301880753241 ± 1.5e-07
E     comparison failed
E     Obtained: 0.1361302885494818
E     Expected: 0.1461301880753241 ± 1.5e-07
FAILED tests/test_validation.py::test_validation[2bin_2channel_couplednorm_mu1]
1 failed in 1.86s
```

- **My findings:** The issue (opened against commit `8c4d1ba` in January 2021) still holds on today's `main`. The file is 1024 lines, there's no walkthrough in it or in `docs/`, and neither pytest's `--setup-plan` nor a failure traceback names the fixture that holds the failing configuration's data. The structure is consistent: 9 configurations, each with a `source_`/`spec_`/`expected_result_` fixture trio, tied together by the `setup` parametrize list (line 750). That makes a walkthrough feasible, but the issue leaves walkthrough vs restructure open, so I asked the maintainer in my claim.

**Toolkit**

- Tool: repro-check (with its real-repo scope installed this week), run as `claude "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue ..."`, plus my voice guard's `--draft` check on both files.
- What it produced: on the claim alone it said reject, failing "claim-specific" over "I'll post what I find here by Sunday, November 22" because my rubric says "no date". On the claim plus repro it said accept, with all six required checks passing, but it flagged that my step-6 output (`1 failed, 9 passed`) didn't match the single test ID the step named, and that two sentences had no output behind them. A second run said accept and caught that my AI disclosure contradicted itself.
- Difficulties: my repro-check rubric and my voice guide disagree. The rubric bans any date in a claim, while my guide (and this week's assignment) asks for a report-back date. The grader noticed and passed the second run anyway, but a literal read still fails it.
- What changed: I re-ran the single test ID and pasted its exact output, deleted the unrun `--group dev` claim, rewrote the AI-disclosure line to say what actually happened, and re-ran the steps myself after the voice guard stopped "reproduced" for output Claude had run. I left the rubric as it is: the date is the only thing a literal read fails, the date is for an update and not a result, and the assignment asks for it, so I accepted that one fail and posted the claim with the date.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
