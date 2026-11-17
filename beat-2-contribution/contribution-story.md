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
**Pull Request:** https://github.com/scikit-hep/pyhf/pull/2800

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

**Unit 7: Installed the ship-gate and wrote a Windows-aware ship manifest for pyhf (2026-11-16)**

My manifest has three rows taken from pyhf's `ci.yml` and `.pre-commit-config.yaml` (the core pytest command, `ruff check` and `ruff format --check`), and on my branch the ship-gate passed all three and then my pr-precheck. Its only notes were three voice problems in my description, including an unsupported "CI runs on Linux and macOS". What fought me was Windows: the gate runs rows in `cmd.exe`, so `.venv/Scripts/ruff` failed with `'.venv' is not recognized` until I switched to backslashes, and CI's own test command already gave `21 failed ... 7 errors` on untouched `main` because of Windows temp paths and git symlinks. I moved `tests/test_scripts.py` and `tests/test_patchset.py` to the second table with the counts and causes, and checking that voice note turned up pyhf's own `ci-windows.yml` skipping `test_scripts.py` too, so I cited it. I didn't change my rubric: none of its checks asked for anything pyhf doesn't.

**Unit 8: Installed the PR monitor and wrote a playbook from pyhf's real review timing (2026-11-17)**

My playbook's staleness rule comes from six recent merged PRs from outside contributors: each got a first response either within a day (#2720, #2668, #2580) or after 59-89 days (#2711, #2716, #2710). So I set a 30-day wait before a single nudge, and no nudge at all on a draft or after a maintainer says they won't look at it. On PR #2800 the monitor's state said `FEEDBACK-IN`, but the only comment was CodeRabbit's bot summary. My Response rule 2 (a bot summary isn't review feedback) made it draft "No reply" and a note to wait for matthewfeickert. That note was wrong: he'd already said no on #1250 that morning, and the monitor reads only the PR thread ("I can't see #1250 from this report"). I didn't follow the draft. I followed my Walk-away criteria and closed the PR myself, and I'd add reading the linked issue's comments to the monitor so the decision wouldn't be missed.

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

Nothing is crashing. What's missing is an explanation. `tests/test_validation.py` checks pyhf's CLs results against reference numbers for 9 model configurations, and to avoid repeating code it builds each configuration out of three fixtures that a single `setup` fixture looks up by name. That keeps the file short, but when a validation test fails, nothing tells you which configuration it was or where its data lives, so you have to read through the file to find out.

**Expected Behavior**

Someone looking at a failing ID like `test_validation[2bin_2channel_couplednorm_mu1]` should be able to get to the fixtures that define it (`source_`, `spec_` and `expected_result_2bin_2channel_couplednorm`) without reading `setup` and working out an f-string. They should also be able to tell what each value in the `setup` parameter list means, and how to add a new configuration.

**Current Behavior**

On `main` at `4bbcb86`, the file has no module docstring and nothing in `docs/` mentions it. `pytest --setup-plan` lists only `request, reset_backend, reset_events, setup, setup_cuda_environment`, and a forced failure's traceback names only lines 795 (`test_validation`) and 658 (`validate_hypotest`), never the fixture at line 443 that holds the expected value. The IDs don't always match the fixture names either: `1bin_shapesys_q0_mu1_toys` is `1bin_shapesys_q0_toys`, and that one's source and spec are aliases at lines 72 and 107.

**Affected Components**

- `tests/test_validation.py`: the 9 fixture trios (lines 13-631), `validate_hypotest` (632), the parametrized `setup` fixture and its `ids` list (661-769), and `test_validation` (771)
- `validation/data/*.json`: the inputs the `source_*` fixtures load
- `docs/development.rst`: the "Running with pytest" section, where developers look for test instructions

**Toolkit**

- Tool: Claude Code in my pyhf clone, with my `CLAUDE.local.md` loaded, used to trace how `setup` reaches the fixtures and to check each fixture name against the `ids` list; plus `grep` and `pytest --setup-plan` myself.
- What it produced: the ID-to-name-to-fixture map for all 10 test IDs, and one fact my unit 6 notes had wrong: `1bin_shapesys_q0_toys` has no `source_`/`spec_` fixtures of its own, only module-level aliases at lines 72 and 107.
- Difficulties: my `CLAUDE.local.md` said the `_toys` case only had "an extra `expected_result_..._toys`", which is true but leaves out the aliases, and a walkthrough built from that note would have pointed readers at fixtures that don't exist.
- What changed: I grepped for `q0_toys` before writing the plan, put the aliases into Understanding the Issue and plan.md, and the walkthrough now has to explain them.

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

On `main` at `4bbcb86`, all the "hunting down" comes from one place in `tests/test_validation.py`:

- The file defines 9 configurations, each as three module-scoped fixtures named by convention: `source_<name>` (loads `validation/data/*.json`), `spec_<name>` (builds the pyhf model spec from the source) and `expected_result_<name>` (the expected CLs values), in lines 13-631. `1bin_shapesys_q0_toys` has no source or spec fixtures of its own. Lines 72 and 107 alias them to the `q0` ones, and only `expected_result_1bin_shapesys_q0_toys` (line 125) is separate.
- The `setup` fixture (line 750) is parametrized with 10 tuples `(name, config_counts, mu, test_stat, tolerance, calctype)` (lines 661-749) and a parallel `ids` list. It fetches the trio by string at lines 752-754: `request.getfixturevalue(f"source_{_name}")` and the matching `spec_`/`expected_result_` lines.
- Because the trio is fetched by string, pytest never sees it as a dependency. `--setup-plan` lists only `request, reset_backend, reset_events, setup, setup_cuda_environment`, and a forced failure's traceback names only line 795 (`test_validation`) and line 658 (the assert in `validate_hypotest`), never the fixture at line 443.
- The IDs aren't the fixture names (`1bin_shapesys_q0_mu1_toys` is `1bin_shapesys_q0_toys`), so the only way from an ID to a name is to find the ID's position in `ids` and read the tuple at the same position.
- None of this is written down anywhere: the file has no module docstring, and `grep -rln "test_validation" docs README.rst CONTRIBUTING.md` prints nothing.

So the cause is that the mapping from ID to name to fixture, and what each tuple field means, exist only implicitly in the code.

**Proposed Solution**

The issue offers "either revise all of it or include some sort of walkthrough of how things work." I asked matthewfeickert which he'd prefer, and he hasn't answered yet. I'm building the walkthrough because it changes no test logic and is easy to replace if he wants a restructure, and the PR goes up as a draft that says so. In scope: a module docstring at the top of `tests/test_validation.py` (the fixture trio convention, the `_toys` aliases, what each `setup` tuple field means, how to get from a failing ID to its fixtures, how to run one case, how to add a configuration); a short "Validation tests" subsection in `docs/development.rst` under "Running with pytest" that points to it; and my name at the bottom of `docs/contributors.rst`, which CONTRIBUTING.md asks of every PR.

Not changing: the fixtures or how `setup` fetches them, any expected value or tolerance, `validate_hypotest` and its assert messages, and the other tests in the file. Failure tracebacks will look the same; the file will explain how to read them.

**Implementation Plan**

1. Create branch `docs/1250-test-validation-walkthrough` from `main`. CONTRIBUTING.md names no branch format, and the repo's documentation branches use a `docs/` prefix (`docs/use-furo`).
2. In `tests/test_validation.py`, add the module docstring as the first statement of the file, with lines at most 88 characters (ruff `line-length`). Commit `docs: Add walkthrough docstring to test_validation module`.
3. In `docs/development.rst`, add the pointer subsection after the `nox` block under "Running with pytest". Commit `docs: Point to test_validation walkthrough in development guide`.
4. Append `- Adarsh Kessani` to `docs/contributors.rst`. Commit `docs: Add Adarsh Kessani to contributors`.
5. Run the checks in my ship manifest and the test plan (see Testing Strategy), and save the output in `test_evidence.md`.

No new test: the change is documentation only and changes no behavior, so there's no failing case for a test to cover. The 20 existing tests in the file must still pass.

Deviations (2026-11-16): the docs addition is a `^^^` "Validation tests" subsection rather than a bare paragraph, to match its sibling "Contrib module matplotlib image tests". My manifest's test row also adds `--ignore tests/test_scripts.py --ignore tests/test_patchset.py`, because both files fail on `main` in my Windows clone (see Testing Strategy). Neither changes what the diff does. On 2026-11-17 I rebased onto upstream `main` at `a19e98a3`, which doesn't touch the three files, so the line numbers above still hold.

**Toolkit**

- Tool: plan-check, run as `claude "plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue https://github.com/scikit-hep/pyhf/issues/1250"` from my clone.
- What it produced: accept, with all 8 required checks passing and the preferred one too. It checked my line numbers against the repo and confirmed them ("the value at 451 is `exp[1]`, so the forced failure lands on the assert at 658"). It also flagged that I was breaking my own `CLAUDE.local.md` rule to wait for the maintainer's answer before building, that "I'll build the walkthrough option as a draft PR" broke my voice guide's no-promises rule, that my comment didn't open with matthewfeickert's suggestion, and that two of my claims were unverified.
- Difficulties: the first run graded nothing. Headless `claude -p` can't answer permission prompts, so it couldn't read `rubric.md` or `procedure.md` in `~/.claude/skills/plan-check/`. It also told me 102 stray `UsersAdarshAppDataLocalTemppytest-of-*` files were in my clone root, left by my own test run.
- What changed: I reran it with `--add-dir` for the skill folder and read-only tools allowed. Then I rewrote the comment to open with matthewfeickert's "either revise all of it or include some sort of walkthrough", changed "I'll build" to "I'd draft", backed the branch-name claim with real branch names, marked the notebook claim as an assumption, and deleted the stray files before branching. I kept building before an answer, because the PR is a draft and the walkthrough is easy to drop, and the PR description says the question is still open.

---

## Testing Strategy

This is a documentation fix, so there's no bug output that goes away. The checks prove three things: the walkthrough now exists and can be found, it's correct about the code, and nothing else changed. Everything below ran on Windows 11 with Python 3.14.7, on branch `docs/1250-test-validation-walkthrough` (`95597859`) against `main` (`4bbcb86`), on 2026-11-16. On 2026-11-17 I rebased onto upstream `main` at `a19e98a3` (branch now `07588f88`) and re-ran steps 1-5, which gave the same output: `20 passed`, the same forced-failure traceback at lines 881 and 744, and `1064 passed, 13 skipped, 2 xfailed` for the core suite.

1. **Repro step 2, before and after.** `grep -rln "test_validation" docs README.rst CONTRIBUTING.md` printed nothing on `main` (exit 1) and printed `docs/development.rst` on the branch. `ast.get_docstring` on the test file printed `None` on `main` and `Validation tests: pyhf results compared with reference results.` on the branch.
2. **Repro step 5, re-run on the branch.** I made the same forced change to `expected_result_2bin_2channel_couplednorm` (now line 537), and the traceback still named only `test_validation` (881) and `validate_hypotest` (744), with `Obtained: 0.1361302885494818` / `Expected: 0.1461301880753241 ± 1.5e-07`, the same as on `main`. That's what the plan said would happen, since the fixtures aren't restructured. Following the docstring from the ID, `grep -n "def expected_result_2bin_2channel_couplednorm"` found line 529, the fixture I'd changed. After the revert, `git status --short` printed nothing.
3. **The walkthrough matches the code.** An `ast` script read `setup`'s `params` and `ids` and printed `all 3 fixtures found` for all 10 IDs, including `1bin_shapesys_q0_mu1_toys` through its aliases. The docstring's command `pytest tests/test_validation.py -k 2bin_2channel_couplednorm_mu1 --collect-only -q` printed `1/20 tests collected (19 deselected)`. Its "-2 to +2 sigma" wording matches `return_expected_set` in `src/pyhf/infer/__init__.py` line 67.
4. **Unchanged behavior.** `pytest tests/test_validation.py -q` printed `20 passed in 24.79s`, the same count as on `main`.
5. **Repo checks from my ship manifest**, run through `cmd.exe` the way the ship-gate runs them. The test command gave `1064 passed, 13 skipped, 2 xfailed`, the same as on `main`. `ruff check` printed `All checks passed!` and `ruff format --check` printed `155 files already formatted`. Before writing the manifest, I ran CI's own test command on untouched `main` and got `21 failed, 1121 passed, 13 skipped, 2 xfailed, 7 errors`. All of those failures are in `tests/test_scripts.py` (19 `FileNotFoundError`s on temp paths with the backslashes stripped, `'C:UsersAdarshAppDataLocalTemp...'`) and `tests/test_patchset.py` (2 failures and 7 errors, `JSONDecodeError` on fixture JSONs that are git symlinks, which Windows checks out as text). Those two files went in the manifest's second table with the date and the reason, and so did the doctests and the notebook suite.

**Toolkit**

- Tool: my ship manifest's three checks, which I ran through Python's `subprocess.run(cmd, shell=True)`, the same call `check_gate.py` makes, so I'd see exactly what the gate would see. I also ran the repro steps and an `ast` script that checks the docstring against the code.
- What it produced: before/after pairs for repro steps 2 and 5, `all 3 fixtures found` for all 10 IDs, and a manifest test row that passes with the same `1064 passed` count on `main` and on the branch.
- Difficulties: on Windows the gate runs rows in `cmd.exe`, not Git Bash, so the assignment's example form `.venv/bin/pytest` doesn't exist here, and even `.venv/Scripts/ruff` failed with `'.venv' is not recognized`. CI's test command also failed on untouched `main` (30 failures and errors), and its failing runs dropped 102 stray files into my clone root.
- What changed: every manifest row uses `.venv\Scripts\...` with backslashes. I narrowed the test row with two `--ignore`s, recorded both files in the second table with the counts and causes, and wrote both into `test_evidence.md` and the PR description so a maintainer can see what wasn't run and why.

---

## Implementation Notes

**2026-11-16 (setup)**

Installed the ship-gate and wrote my manifest. matthewfeickert hadn't answered my walkthrough-vs-restructure question, so I decided to build the walkthrough as a draft PR: it touches no test logic and is easy to drop. My `CLAUDE.local.md` said to wait for an answer first. I decided against waiting because a draft PR is where that question can still be answered, and the PR description says it's open. Before writing the manifest, I ran CI's test command on untouched `main`, which turned up 30 failures and errors in `test_scripts.py` and `test_patchset.py`, all from Windows paths and git symlinks. That's where most of the session went, and it's why those two files sit in the manifest's second table.

**2026-11-16 (plan)**

While checking the `setup` params, I found `"1bin_shapesys_q0_toys"` had no `source_` or `spec_` fixture of its own. Grepping for `q0_toys` showed the module-level aliases at lines 72 and 107. My unit 6 notes only mentioned the extra `expected_result_`, so a walkthrough built from them would have been wrong for that case. plan-check accepted the plan on its second run, and I fixed the voice-guide notes it raised.

**2026-11-16 (build)**

Made three commits on `docs/1250-test-validation-walkthrough`: the 86-line docstring, the "Validation tests" subsection in `docs/development.rst` (a `^^^` heading to match its sibling, instead of the bare paragraph I'd planned), and my name in `docs/contributors.rst`. I kept every docstring line at 88 characters or less, and swapped the run example from a full test ID (over 88 characters) to `-k 2bin_2channel_couplednorm_mu1`, after checking that it selects exactly 1 of 20 tests. Git warned about LF to CRLF in the working copy, so I checked the committed diff for carriage returns: there were 0.

**2026-11-16 (draft)**

The first draft of my PR description said "I ran every command in Testing myself", which wasn't true: Claude ran them in this session. That's the same mistake my voice guard caught in unit 6. I rewrote the AI-use section to say what actually happened before running the voice guard, which then printed `voice-guard: pass`.

**2026-11-16 (ship-gate)**

I re-ran the Testing commands in my own terminal and updated the AI-use line to say I ran them. My first ship-gate run after that stopped on one test, `test_sbottom_regionB_1400_550_60`, with a `KeyError` from `tarfile`. The diff doesn't touch it, and it passed in every re-run, so I left the manifest as it was, and the next gate run passed. Before opening the PR I fetched upstream: `main` had moved 2 commits past `4bbcb86` (`7953abc1` drops Python 3.10, `a19e98a3` moves the Intel macOS CI to nightly). Neither touches my three files, and both workflow claims in my description still hold.

**2026-11-17 (rebase)**

Rebased the branch onto upstream `main` at `a19e98a3`, as CONTRIBUTING.md step 6 asks. It was clean, and the diff is still `3 files changed, 98 insertions(+)`. Upstream had raised some dependency lower bounds, but my installed scipy 1.18.1, numpy 2.5.5 and iminuit 2.33.0 are all above them, and ruff is still pinned at v0.17.3, so the manifest didn't change. The re-run gave the same output as before the rebase. I also added a `## Deviations` section to `plan.md`, which pr-precheck had flagged as missing, and wrote out the two `python -c "..."` commands in full in `pr_draft.md`, which was its last voice note. The next ship-gate run rejected the branch: "Diff stat lists 15 files; 12 (e.g. pyproject.toml, .github/workflows/ci.yml, noxfile.py) are not named by the plan". The gate compares against my local `main`, which I hadn't updated, so upstream's two commits looked like part of my PR. `git fetch upstream main:main` moved local `main` to `a19e98a3`, and `git diff --stat main...HEAD` went back to `3 files changed, 98 insertions(+)`.

**2026-11-17 (PR opened and answered)**

Opened draft PR #2800 at 05:44 UTC through Claude Code. Both hooks passed it, and all six CI checks passed (semantic PR title, CodeFactor, CodeRabbit, Read the Docs, pre-commit.ci). At 07:21 UTC matthewfeickert answered my question on #1250: the walkthrough "as you have implemented it" isn't what he wants, the issue isn't on the roadmap, and the maintainers won't engage further for now. He also asked me to pass on his concern about contributions assigned as coursework. I decided not to rework the PR or argue the decision, and to close it myself.

**2026-11-17 (PR monitor)**

Installed the PR monitor and wrote my playbook. For the staleness rule I measured six recent merged outside PRs: first responses came within a day or after 59-89 days. The monitor's full run drafted "No reply" (the only PR comment was a bot) and a note to wait for the maintainer. It couldn't see the issue thread, where the decision had already been made, so I followed my Walk-away criteria instead.

**Toolkit**

- Tool: Claude Code in my pyhf clone (with `CLAUDE.local.md`), which wrote the docstring and the docs subsection from my plan, made the three commits, and ran each check.
- What it produced: three commits (`8d661238`, `e06b02c1`, `07588f88` after the rebase onto `a19e98a3`; `90a0c906`, `3994cd95`, `95597859` before it), 98 lines added across `tests/test_validation.py`, `docs/development.rst` and `docs/contributors.rst`, and no other files touched.
- Difficulties: the forced-failure line moved from 451 to 537 once the docstring was in, so the repro step had to be redone with the new number. Claude's first AI-use line overstated what I did myself.
- What changed: the repro step in `test_evidence.md` names both line numbers, and the AI-use section now says Claude ran the commands. I still need to rewrite that section in my own words before opening the PR.

---

## Pull Request

**PR Link:** https://github.com/scikit-hep/pyhf/pull/2800 (opened as a draft on 2026-11-17, `docs: Add walkthrough of test_validation fixtures`)

**PR Description:** The description follows pyhf's PR template and starts with `Fixes #1250`. It quotes matthewfeickert's "either revise all of it or include some sort of walkthrough" and says my question about which he'd prefer is still unanswered, so the draft does the walkthrough, which is easy to drop if he wants a restructure. It explains that `setup` fetches each configuration's fixtures by string, so `--setup-plan` and tracebacks never name them. The Changes section lists the three files: the 86-line module docstring in `tests/test_validation.py`, the "Validation tests" subsection in `docs/development.rst`, and my name in `docs/contributors.rst`. It also lists what's not changed (fixtures, the `setup` lookup, expected values, `validate_hypotest`). Testing pastes the before and after of the `grep` and `ast.get_docstring` checks, the forced-failure traceback on `main` (795/658) and the branch (881/744), the docstring lookup that finds line 529, the 10-ID fixture check, `20 passed`, and the three repo checks (`1064 passed, 13 skipped, 2 xfailed`, ruff clean). It notes the re-run after rebasing onto `a19e98a3`. It lists what I didn't run and why: `test_scripts.py` and `test_patchset.py` fail on `main` in my Windows clone (with the `git ls-files` mode `120000` evidence), the doctests, and the notebook, contrib and benchmark suites. It ends with an AI-use section and the template checklist, with Assignee marked N/A and the reason.

**Maintainer Feedback:**

- 2026-11-17: matthewfeickert answered on #1250 (not on the PR), about 90 minutes after I opened it. To my walkthrough question he wrote "As you have implemented it in PR #2800, no", said the issue "is not a high priority" and isn't on any project board, and said "we are not going to engage on this further for the foreseeable future", though the PR "could be refactored into something useful". He also asked me to pass on to my course that assigning open source contributions without the maintainers' consent in advance puts a load on them. The only comment on the PR itself is CodeRabbit's automated summary ("No actionable comments were generated").
- 2026-11-17: my response: @matthewfeickert, thank you for the direct answer, and for the advice on choosing issues where I have relevant experience and working out the goals with maintainers first. I'm closing #2800 so it isn't in your review queue.
To clarify my position, I chose the issue myself rather than it being a specific issue assigned to me by an instructor. I do apologize for the miscommunication. 

**Status:** Closed

**Toolkit**

- Tool: the ship-gate, run by hand as `python3 ~/.claude/ship-gate/check_gate.py --branch --title "docs: Add walkthrough of test_validation fixtures" --body-file pr_draft.md`, which runs my manifest's three checks and then my pr-precheck on the branch, plan, evidence and description.
- What it produced: `ship-gate: PASS`. The core test suite was ok (148s), ruff lint ok, ruff format ok, and pr-precheck passed all 7 required checks plus the title check, for example "Description claims match diff [pass]: '86 lines, no code changes' matches +86 in test_validation.py". It left three voice notes: my description quoted the docstring's run command with `--collect-only -q` added without saying so, summarized the `ast` check instead of pasting it, and said "CI here runs on Linux and macOS" with nothing behind it.
- Difficulties: the hook test printed only one stop under headless `claude -p`. The voice guard's log showed it had also fired, but Claude only quoted the ship-gate's message. Getting the manifest to pass at all took most of the session, because on Windows the gate runs rows in `cmd.exe` and CI's own test command fails on `main` here. My own first runs failed for a plainer reason: I started the gate from `codepath` and then from `ai301-unit7-starter`, so it couldn't find `pr_draft.md` and said "is not inside a git clone". It only works from inside the clone. On my second real run it stopped with `1 failed, 1063 passed`: `tests/test_regression.py::test_sbottom_regionB_1400_550_60` hit a `KeyError` from `tarfile`, which my diff doesn't touch. That test then passed alone, with `test_paramviewer.py` (`13 passed`), and in a full run of the same command (`1064 passed`). The archive it reads was intact and hadn't changed since it was first downloaded, so I couldn't find a cause.
- What changed: I fixed all three voice notes in `pr_draft.md`. Checking the CI claim led me to `.github/workflows/ci-windows.yml`, which runs pytest with `--ignore tests/test_scripts.py`, the same exclusion my manifest makes, so I cited it in the description and in the manifest's reason. I re-ran the Testing commands myself and changed the AI-use line to say so. I kept the regression test in the first table instead of moving it to the second, because it passes on my branch and the second table is for checks that fail on `main`. The next gate run printed `ship-gate: PASS` (core suite ok in 184s, all 7 required pr-precheck checks pass), with one voice note left: two commands shortened to `python -c "..."` in the description.

---

## Learnings and Reflections

**Technical Skills Gained**

- **pytest fixtures:** how a parametrized fixture can look up other fixtures by string with `request.getfixturevalue`, why that hides them from `--setup-plan` and tracebacks, and how to trace a failing test ID back to its data with `-k`, `--collect-only` and `--setup-plan`.
- **Python's `ast` module:** reading a test file's structure without running it, which I used to check all 10 `setup` IDs against their fixtures and to print a module's docstring.
- **Git on Windows:** files stored as symlinks (mode `120000`, checked with `git ls-files -s`) come out as one-line text files when `core.symlinks=false`, which broke `src/conftest.py`, three patchset fixtures, ruff, and the doctests in my clone.
- **Rebasing a feature branch onto upstream `main`,** and learning that tools comparing against my local `main` need it fast-forwarded too (`git fetch upstream main:main`).
- **Reading a project's CI,** such as `ci.yml`, `ci-windows.yml` and `.pre-commit-config.yaml`, to find the checks it actually runs, and writing them so they work in `cmd.exe`, which is how the ship-gate runs them.

**Challenges Overcome**

- **Installing pyhf:** `.[xmlio]` alone died on an `iminuit` warning filter, and the conftest needed `jax` even to run one file. `.[all]` fixed both.
- **A failing test suite on `main`:** CI's own test command gave `21 failed ... 7 errors` on untouched `main` in my clone. I traced every failure to Windows temp paths or git symlinks, moved both files to my manifest's second table with the reasons, and found that pyhf's `ci-windows.yml` already skips `test_scripts.py`.
- **A one-off test failure:** a ship-gate run stopped on `test_sbottom_regionB_1400_550_60`, which my diff doesn't touch. I re-ran it alone, with the other test that reads the same archive, and in full runs before deciding not to hide it in the second table.
- **Being honest about AI help:** my voice guard and I caught the same mistake twice, a line saying I ran commands that Claude had run. Both times I re-ran the commands myself and rewrote the line.

**What I'd Do Differently Next Time**

I'd wait for the maintainer's answer before building. My own `CLAUDE.local.md` said to wait, and plan-check flagged that I was breaking that rule. I went ahead because the PR was a draft and easy to drop, and matthewfeickert's "no" came about 90 minutes after I opened it. I'd also pick an issue differently. My scout passed a nearly six-year-old issue on repo activity alone, and the maintainer's advice was to choose issues where I have relevant experience and first work out with the maintainers whether the work is wanted and what it should aim for. A scout check on issue age and on whether the issue is on a project board would have caught that this one wasn't a priority. Finally, I'd ask before contributing as coursework. The maintainer pointed out that the maintainers never agreed to take part in this kind of assignment.

---

## Resources Used

- pyhf's CONTRIBUTING.md (https://github.com/scikit-hep/pyhf/blob/main/CONTRIBUTING.md): Conventional Commits, draft PRs for multi-commit work, the `docs/contributors.rst` entry, and the test command.
- pyhf's CI configuration, `.github/workflows/ci.yml` and `.github/workflows/ci-windows.yml` (https://github.com/scikit-hep/pyhf/tree/main/.github/workflows), and `.pre-commit-config.yaml`: the exact pytest arguments and ruff version for my manifest, and evidence that `test_scripts.py` is skipped on Windows upstream too.
- pyhf's "Developing" page (https://scikit-hep.org/pyhf/development.html), whose "Running with pytest" section is where my docs pointer went.
- The issue thread, scikit-hep/pyhf#1250 (https://github.com/scikit-hep/pyhf/issues/1250): matthewfeickert's original description of the "hunting down" problem, and his reply to my claim, which was the most useful feedback I got all term on how to choose and approach an issue.