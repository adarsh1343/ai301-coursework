# Toolkit Portfolio

**Student:** Adarsh Kessani

## Featured Tools

My toolkit is a set of checks that grade my own open source work before a maintainer has to read it. They check whether an issue is worth taking, whether my reproduction and plan hold up, whether my PR does what my plan said, and whether my words claim more than my output shows. I'm featuring these four because each one changed after it got something wrong on real input, and each change shows up in what happened next. The ship-gate comes first, because it's where everything else came together on my real PR to scikit-hep/pyhf.

**ship-gate (with my ship manifest)**

**What it is, and the judgment hole I wrote:** A Claude Code hook that runs before `gh pr create`. It runs the project's own checks from my ship manifest, then my pr-precheck rubric on the branch, and it blocks the PR if either fails. I wrote the manifest: which of pyhf's checks count, how each one runs on my Windows machine, and which ones I exclude, each with a dated reason.

**How it evolved, dated:**
- 2026-11-16: I took the test command straight from pyhf's `ci.yml` and ran it on untouched `main` first. It failed with 21 failures and 7 errors, all in `test_scripts.py` (Windows temp paths) and `test_patchset.py` (git symlinks checked out as text). I narrowed the row with two `--ignore`s and moved both files to the excluded table with the reason. I also rewrote every row with backslashes, because the gate runs them in `cmd.exe`.
- 2026-11-17: after I rebased my branch, the gate rejected it with a 15-file diff. It compares against my local `main`, which I hadn't updated. I fast-forwarded it with `git fetch upstream main:main` and ran the gate again.

**Evidence:**

On untouched `main`, before I changed the manifest:

```text
21 failed, 1121 passed, 13 skipped, 2 xfailed, 7 errors in 150.18s
```

The manifest's first table afterwards:

| Check | Command |
|---|---|
| Core test suite | `.venv\Scripts\python -m pytest --ignore tests/contrib --ignore tests/benchmarks --ignore tests/test_notebooks.py --ignore tests/test_scripts.py --ignore tests/test_patchset.py` |
| Ruff lint | `.venv\Scripts\ruff check . --exclude src/conftest.py` |
| Ruff format | `.venv\Scripts\ruff format --check . --exclude src/conftest.py` |

The run that let PR #2800 open, from `gate-log.md` on 2026-11-17 at 00:43:

```text
ship-gate: PASS. docs/1250-test-validation-walkthrough passed your manifest's checks and your pr-precheck.
  repo check Core test suite: ok (184s)
```

**pr-precheck**

**What it is, and the judgment hole I wrote:** A skill that grades a PR package (the diff, the description, the test evidence) against the plan it claims to carry out. I wrote its rubric: 7 required checks and 1 preferred, including "Diff stays inside plan", "Repro before and after shown", and "Description claims match diff".

**How it evolved, dated:**
- Unit 4 (October 2026): my first rubric agreed with only 13 of 20 eval packages, and it rejected all seven clear-accept packages. "Repro before and after shown" demanded a command and printed output for every step, which failed a terminal UI with nothing to print (pkg-08, lazygit) and results written beside commands. I rewrote it to accept keys pressed and text seen, or a result in a comment, while still failing a repro that was never re-run.
- 2026-11-16/17 (unit 7): its scope moved from the sandbox to real repos, and it ran inside the ship-gate on my pyhf branch.

**Evidence:** The eval runs went from 13/20 (with clear-accepts at 0/7), to 7/7, to 20/20 with the rubric I uploaded. On my real branch it caught a diff that wasn't mine (gate log, 2026-11-17 00:32):

```text
Diff stays inside plan [fail]: Diff stat lists 15 files; 12 (e.g. pyproject.toml,
.github/workflows/ci.yml, noxfile.py) are not named by the plan or recorded in deviations.
```

**voice guard (with my voice guide)**

**What it is, and the judgment hole I wrote:** A hook that checks anything I'm about to post on GitHub (an issue comment, a PR description) against my voice guide, and stops it if it breaks a rule. I wrote the guide, including its rules against claiming output I didn't produce ("Paste results as they printed", "No guessing at the cause"), and in unit 6 an Upstream section for claims, questions, and pings.

**How it evolved, dated:**
- 2026-11-05: built the guide, and the first manual checks stopped on "Be specific".
- 2026-11-15 (unit 6): added the Upstream section and hooked the guard into Claude Code. The wiring test showed that a `"matcher": "Bash"` hook never sees Claude Code's PowerShell tool on Windows.
- 2026-11-15 to 2026-11-17: it stopped my claim, my repro report, and my PR draft. Each time I rewrote the draft to say only what my output showed.

**Evidence:** Stops from `guard-log.md`:

```text
2026-11-15 19:39 [manual] stop: rule broken: "A claim of "reproduced" for output I did not run myself, or that shows a different behavior from the issue" | target: repro-draft.md
2026-11-17 00:23 [manual] stop: rule broken: "I report what I ran and what I observed. I do not say why it happens unless my output shows it." | target: pr_draft.md
```

After the first stop, I re-ran the repro steps myself before posting my claim. After the second, I replaced the sentence about Windows checking out symlinks as text with what `git ls-files -s` actually printed (mode `120000`).

**issue-scout**

**What it is, and the judgment hole I wrote:** A skill that searches GitHub for first-contribution issues, filters and ranks them, then grades the top candidates. It grew out of my unit 1 issue-select skill. I wrote its search strategy (three `gh search issues` queries from my fit profile) and its rubric ("Maintainer alive", "Repo in use", "Nobody else already on it", and others).

**How it evolved, dated:** The changes, from `scan-notes.md`:
- 2026-11-04: all three queries returned 0 results, because quoting `"docker OR deploy OR ci"` made it one exact phrase. I removed the quotes.
- 2026-11-04: ros-envs#3 passed "Repo in use" on a release from a 2-day-old, one-contributor repo, and whyfull#28 got through the closed-PR route. I added a 30-day minimum repo age to both routes, and counted PR reviews as maintainer activity after Canvas-Web#290 failed "Maintainer alive" even though its maintainer was reviewing PRs.
- 2026-11-15: applied the unit 6 scope patch, replacing "Scope fits a newcomer" with a required "scope-feasible" check.

**Evidence:** From `scan-notes.md`, 2026-11-04:

> ros-envs#3 passed 'Repo in use' on a release from a 2-day-old, single-contributor repo. Tightened that check in rubric.md: a release alone doesn't count for repos under 30 days old or with one contributor.

On the 2026-11-15 re-grade, it rejected whyfull#28, which by then had an open PR #38 saying "Fixes #28", and it accepted scikit-hep/pyhf#1250 with all five required checks passing.

## Inventory

- **issue-select (unit 1):** my first grading rubric for a single issue (four required checks), with an eval agreement of 1/1. It became issue-scout in unit 5.
- **repro-check (unit 2):** grades a claim and repro report. I rewrote its `behavior-matches-issue` check twice after it rejected honest "cannot reproduce" reports (pkg-09, pkg-10). The final run agreed 20/20. In unit 6 it accepted my real pyhf repro.
- **plan-check (unit 3):** grades a plan and plan comment. I added a required "Thread-aligned" check after the sample rubric missed a collaborator's comment (calib-03). On 2026-11-16 it accepted my real pyhf plan and flagged that I was building before the maintainer had answered.
- **PR monitor (unit 8):** reads my PR's checks and comments and drafts my next move from my playbook. On PR #2800 it correctly drafted "No reply" to a bot comment, but it can't see the linked issue thread.

## The Arc

**Beat 1**
Date: late September 2026
Friction: my repro-check rubric rejected honest "cannot reproduce" reports (pkg-09, sharkdp/fd#2033), because `behavior-matches-issue` required the artifact to show the issue's symptom.
Change: I rewrote the check twice, ending with a good-faith attempt at the issue's scenario, with the differences stated.
Result: the final eval run agreed 20/20, and wrong-target packages (pkg-02, pkg-08) were still rejected.

**Beat 2**
Date: October 2026
Friction: my first pr-precheck rubric agreed with only 13 of 20 packages and rejected every clear-accept package, because it demanded printed output for every step.
Change: I rewrote "Repro before and after shown" to accept keys pressed and text seen, or a result written beside a command, while still failing a repro that was never re-run.
Result: the next run agreed 7/7 on clear-accepts and the final run 20/20, and the same check passed my real pyhf PR in unit 7.

**Beat 3**
Date: 2026-11-04
Friction: my scout's queries returned 0 results, and once fixed, its rubric passed brand-new repos (ros-envs#3, whyfull#28).
Change: I removed the quotes from the queries, added a 30-day repo-age minimum to both "Repo in use" routes, and counted PR reviews as maintainer activity.
Result: on the 2026-11-15 re-grade it rejected whyfull#28 (claimed by PR #38) and accepted pyhf#1250, the issue I worked on.

**Beat 4**
Date: 2026-11-15
Friction: my voice guard stopped my repro report for claiming "reproduced" on output Claude had run, not me.
Change: I re-ran the repro steps myself and quoted the printed output in my claim.
Result: my claim was posted on pyhf#1250 with my own output in it, and the maintainer answered it on 2026-11-17.

**Beat 5**
Date: 2026-11-16
Friction: pyhf's own CI test command failed on untouched `main` in my Windows clone (21 failed, 7 errors), so a manifest built from it would block every PR.
Change: I narrowed the test row with two `--ignore`s and moved both files to the excluded table with dated reasons.
Result: the ship-gate passed my branch, and I later found pyhf's own `ci-windows.yml` makes the same `test_scripts.py` exclusion.

**Beat 6**
Date: 2026-11-17
Friction: pr-precheck rejected my rebased branch with a 15-file diff that included upstream's commits.
Change: I fast-forwarded my local `main` to upstream with `git fetch upstream main:main`.
Result: the diff went back to 3 files, the gate printed PASS, and PR #2800 opened with all six of its GitHub checks passing.

**Beat 7**
Date: 2026-11-17
Friction: the maintainer said no to my approach on the issue thread, but my PR monitor reads only the PR thread, so it drafted a note to wait for him.
Change: I didn't follow the draft. I followed my playbook's walk-away rule instead.
Result: I replied on #1250 and closed PR #2800 myself the same day.

## Limitations and What's Next

The biggest limit wasn't in a tool. It was how I used them. plan-check flagged that I was building before the maintainer had answered my question, and I went ahead anyway because the PR was a draft. His answer, about 90 minutes after I opened it, was "As you have implemented it in PR #2800, no." My tools check whether my work is accurate and well-supported. None of them checks whether the maintainers want it. Next I'd add an issue-age check and a "is it on a project board" check to the scout, and treat plan-check's "wait for the maintainer" note as a stop, not a suggestion. I'd also have the ship-gate compare against `upstream/main` so a stale local branch can't fool it, and have the PR monitor read the linked issue's comments.

**Tool limits.**

ship-gate: my manifest only fits my Windows clone, so it never runs `test_scripts.py`, `test_patchset.py`, or the doctests that pyhf's Linux CI runs, and it trusts my local `main` as the base.
pr-precheck: it only judges the diff it's shown, so a stale base makes it reject a correct branch, and it can't tell whether the maintainers want the change at all.
voice guard: the hook only matches Claude Code's Bash tool, so a post through the PowerShell tool on Windows skips it, and its grading isn't fully consistent (it passed a line in one run and stopped the same line in a later run).
issue-scout: it judges the repo's activity, not the issue's, so it passed a nearly six-year-old issue that wasn't on the project's roadmap.
