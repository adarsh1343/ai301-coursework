# Documenting pyhf's validation tests, and the ship-gate that checked my PR before it opened

## Overview

pyhf is a Python library for the statistical models used in particle physics. Its issue [#1250](https://github.com/scikit-hep/pyhf/issues/1250) said that when a test in `tests/test_validation.py` fails, you have to go "hunting down" which configuration failed and where its data lives. I reproduced the problem, wrote an 86-line walkthrough docstring for the test module plus a pointer in the developer docs, and opened it as draft [PR #2800](https://github.com/scikit-hep/pyhf/pull/2800). Before the PR could open, it had to pass my ship-gate: a hook that runs the project's own checks from a manifest I wrote, then my pr-precheck rubric from unit 4, and blocks the PR if either fails. The gate stopped my branch twice, and I changed my setup both times. The PR opened with every upstream check green. The maintainer then answered that the walkthrough, as I'd built it, wasn't what he wanted, and that the issue isn't on the roadmap, so I closed the PR myself.

## What happened

### The bug, reproduced

The test file builds each of its 9 model configurations from three fixtures, `source_<name>`, `spec_<name>` and `expected_result_<name>`. One `setup` fixture looks them up by string with `request.getfixturevalue(f"source_{_name}")`, so pytest never sees them as dependencies. To show what that costs, I changed one expected value and ran the test. The traceback names two functions and never the fixture that holds the number:

```text
tests\test_validation.py:795: in test_validation
    validate_hypotest(
tests\test_validation.py:658: in validate_hypotest
    assert result == pytest.approx(expected, rel=tolerance), result
E     Obtained: 0.1361302885494818
E     Expected: 0.1461301880753241 ± 1.5e-07
```

The value I'd changed was at line 451, in `expected_result_2bin_2channel_couplednorm`. Nothing in the output points there.

I asked the maintainer on the issue whether he wanted a walkthrough or a restructure. While I waited, I planned the walkthrough because it changes no test logic. Tracing the IDs for the plan turned up something my own notes had wrong: the `1bin_shapesys_q0_toys` case has no fixtures of its own, only two module-level aliases (`source_1bin_shapesys_q0_toys = source_1bin_shapesys_q0`). The docstring explains them, and a script that parses the file with `ast` confirmed all 10 test IDs map to three real fixtures. Here is part of the docstring:

```python
"""
Finding the fixtures for a failing test
---------------------------------------

1. Find the ID in ``ids``. The tuple at the same position in ``params`` is the
   case that ran, and its first entry is the name,
   ``"2bin_2channel_couplednorm"``.
2. The case's data is in ``source_2bin_2channel_couplednorm``,
   ``spec_2bin_2channel_couplednorm`` and
   ``expected_result_2bin_2channel_couplednorm``.
"""
```

### First change: a manifest that tells the truth on Windows

My manifest lists the checks pyhf asks for before a PR. I took the test command straight from pyhf's `ci.yml`. Before trusting it, I ran it on untouched `main`, and it failed:

```text
21 failed, 1121 passed, 13 skipped, 2 xfailed, 7 errors in 150.18s
```

Every failure was in two files. `test_scripts.py` built paths with `shlex.split`, which stripped the backslashes out of Windows temp paths (`'C:UsersAdarshAppDataLocalTemp...'`). `test_patchset.py` read fixture files that git stores as symlinks (mode `120000`), and Windows had checked them out as one-line text files. Neither failure had anything to do with my change. If I'd left the row as it was, the gate would have blocked every PR. If I'd deleted it, it would have checked nothing. So I narrowed it and wrote down why. The test row gained `--ignore tests/test_scripts.py --ignore tests/test_patchset.py`, and both files went into the manifest's "Excluded, with a dated reason" table. I also learned that the gate runs each row in `cmd.exe`, where `.venv/Scripts/ruff` fails with `'.venv' is not recognized`, so every row uses backslashes. Later I found pyhf's own `ci-windows.yml` already runs pytest with `--ignore tests/test_scripts.py`, the same exclusion.

### Second change: pr-precheck rejected a 15-file diff

The gate passed. Then I rebased my branch onto pyhf's newer `main` and ran the gate again. My pr-precheck rejected it:

```text
Diff stays inside plan [fail]: Diff stat lists 15 files; 12 (e.g. pyproject.toml,
.github/workflows/ci.yml, noxfile.py) are not named by the plan or recorded in deviations.
Diff is reviewable [fail]: First offending line: '+name: CI on x86_64 macOS' in new
.github/workflows/ci-macos-intel.yml
```

My branch only touched 3 files, but the gate compares against my local `main`, which I hadn't updated. Upstream's two new commits looked like mine. My rubric was right to reject what it was shown. I moved local `main` forward with `git fetch upstream main:main`, and `git diff --stat main...HEAD` went back to `3 files changed, 98 insertions(+)`. The next run printed:

```text
ship-gate: PASS. docs/1250-test-validation-walkthrough passed your manifest's checks and your pr-precheck.
```

### What landed

I opened the draft PR through Claude Code, and the hook let it through on that pass. On GitHub, all six checks passed: PR title validation, CodeFactor, CodeRabbit, the Read the Docs build of my `.rst` change, and pre-commit.ci. pre-commit.ci ran the full hook set I had excluded locally (`prek run --all-files`, because its fixers rewrite files), and it passed. pyhf's pytest suite didn't run on the PR, so I'm not claiming more than that.

About 90 minutes after I opened it, the maintainer, matthewfeickert, answered my question on the issue: "As you have implemented it in PR #2800, no." He said the issue "is not a high priority", isn't on any project board, and that the PR "could be refactored into something useful" but the maintainers won't engage further for now. I didn't argue the decision. I replied on the issue and closed the PR myself.

Where I used Claude Code: to trace the fixtures, and to run some of the commands. Twice a draft said I'd run commands that Claude had run: my voice guard stopped my repro report for it in unit 6, and the same line turned up in my first PR draft. Both times I re-ran the commands myself before posting.

## Limitations and what's next

My closing note on the PR was "Closing, see my reply on #1250." My reply there began:

> @matthewfeickert, thank you for the direct answer, and for the advice on choosing issues where I have relevant experience and working out the goals with maintainers first. I'm closing #2800 so it isn't in your review queue.

The biggest limit was my process, not my tools: I built and opened the PR before the maintainer answered my question. My own notes said to wait, and plan-check flagged it. Next time I'd wait for the answer, and I'd add two checks to my issue scout: the issue's age (this one was nearly six years old) and whether it's on a project board.

The tools have limits too. My manifest only fits my Windows clone: it skips two test files and the doctests that pyhf's Linux CI runs, so my gate never checks those. The ship-gate also trusts my local `main`, as the rejection showed, so I'd have it compare against `upstream/main`. And my PR monitor, which reads the PR thread to draft my next move, never reads the linked issue. It drafted "wait for matthewfeickert" after he had already said no on #1250. Reading the linked issue's comments is the next change I'd make to it.
