# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

adarsh1343

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5862663154

Hi, I'd like to take this issue as my first contribution to the repo.
The issue says `README.md` tells readers to add `OPENROUTER_API_KEY` to `.env`, while `.env.example` doesn't list that variable and its `LLM_PROVIDER` comment offers only `mock` and `openai`, even though `core/config.py` defines both keys.
Next I will compare `README.md`, `.env.example` and `core/config.py` side by side, and post here exactly what each file says about the API key and `LLM_PROVIDER`, along with the commit I checked. I'm not proposing a change yet.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863156497

Result: reproduced. On the commit below, `README.md` tells the reader to add `OPENROUTER_API_KEY` to `.env`, `.env.example` has no `OPENROUTER_API_KEY` entry and its `LLM_PROVIDER` comment lists only `mock` and `openai`, and `core/config.py` defines the OpenRouter settings. I only compared the three files. I did not run the app, and I did not check which `LLM_PROVIDER` values the code accepts.
Environment: Windows NT 10.0.26200.0, PowerShell, git 2.55.0.windows.3, Python 3.14.7. Cloned from my fork (`adarsh1343/pathreview-ai301-fa26-s1`) at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779` (2026-09-16 14:48:26 -0700, "chore: track five more manifest entries against the tracker"), on branch `main`. After all the steps below, `git status` reported the branch up to date with `origin/main` (my fork) and a clean working tree.
Steps (from the repo root of the clone):

1. What the README says:


```
PS> Select-String -Path README.md -Pattern "OPENROUTER|LLM_PROVIDER|API_KEY|\.env" -Context 2,2
> README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
> README.md:25:cp .env.example .env

```

2. What `.env.example` says about the LLM provider:


```
PS> Get-Content .env.example
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here

```

3. Search `.env.example` for the variable the README names:


```
PS> Select-String -Path .env.example -Pattern "OPENROUTER"
PS>

```

No output: `.env.example` contains no `OPENROUTER` text at all.

4. What `core/config.py` defines:


```
PS> Select-String -Path core\config.py -Pattern "OPENROUTER|OPENAI|LLM_PROVIDER" -Context 1,1
core\config.py:18:    llm_provider: str = Field(default="mock")
core\config.py:19:    openai_api_key: str = Field(default="")
core\config.py:20:    openrouter_api_key: str = Field(default="")
core\config.py:21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
core\config.py:22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")

```

5. Follow the README's copy step and check the result:


```
PS> Copy-Item .env.example .env
PS> Select-String -Path .env -Pattern "OPENROUTER"
PS>

```

No output: the `.env` created by the README's copy step has no `OPENROUTER_API_KEY` entry.
Expected: `.env.example` lists the variables `README.md` tells the reader to set, so copying it gives a `.env` that already has an `OPENROUTER_API_KEY` entry, and its `LLM_PROVIDER` comment matches the providers in `core/config.py`.
Actual: `.env.example` has no `OPENROUTER_API_KEY` line, its comment offers only `mock` and `openai`, and the `.env` copied from it has no `OPENROUTER_API_KEY` entry either, while `README.md:24` and `core/config.py:20` both refer to that key.
I used AI assistance (Claude) while preparing this report, and I ran the commands above myself.

## Eval iterations

**Run history**

12/17, 2/5, 0/3, 3/3, 20/20

- Run 1 (full run): 12/17 scored items agreed. Three packages (pkg-04, pkg-07, pkg-11) errored with `UnicodeEncodeError: 'charmap' codec can't encode character` on Windows; I fixed that with `PYTHONUTF8=1`.
- Run 2 (`--only pkg-03,pkg-05,pkg-09,pkg-10,pkg-12`): 2/5.
- Run 3 (`--only`, 3 packages): 0/3.
- Run 4 (`--only`, 3 packages): 3/3.
- Run 5 (full run, `--save-run eval-run.txt`): 20/20, matching the agreement line in the committed `eval-run.txt`.

**Package analysis**

pkg-09 (sharkdp/fd#2033). The gold label is accept. The gold note reads: "honest cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts, names what differed (uniform name lengths, 2 MiB ARG_MAX) and what a triggering setup likely needs". My first rubric rejected it, failing `behavior-matches-issue`. That check originally said "Pass if the artifact shows the same symptom the issue describes", and the report's output shows all ONE batches before TWO, which is the correct ordering, so the artifact does not show the issue's symptom. A later version said the artifact must show "the issue's scenario was actually run", and that still failed it, because the report never made one command hit the argument-size limit before the other. The report says so itself: "my padding approach may not achieve that". My rubric read the check as "did the artifact reach the issue's condition", when the question that matches the gold label is whether the report made a good-faith attempt at the scenario and honestly recorded what it saw and what differed.

**Check rationale**

`behavior-matches-issue`, as it reads in the rubric I uploaded to `tools/repro-check/`:

> If the report says "cannot reproduce", pass if the steps and artifact are a good-faith attempt at the scenario the issue describes (the same command, layout, or configuration, as far as the attempt's environment allows) and record what was observed instead. The attempt does not have to match every condition of the issue: a different OS, shell, or input is fine, and output that runs fine is expected, as long as the report states the difference.

It reads this way because I revised it twice. The first version required the artifact to show the issue's symptom, which rejected every honest cannot-reproduce report (pkg-09 and pkg-10). The second version required that the trigger was "actually run", which still rejected pkg-09, since it could not create the reported condition. I rejected both in favour of judging whether the attempt was made in good faith at the issue's scenario, with the differences stated. Failures are unchanged: a different scenario, a different failure, or a setup or syntax error still fails, which keeps wrong-target packages like pkg-02 and pkg-08 rejected.

**Trade-offs**

This check gives up strictness about reaching the issue's exact conditions. It accepts a cannot-reproduce report whose attempt fell short of the reported setup (pkg-09 never made one command hit the size limit first, and pkg-10 ran Linux + zsh where the issue used macOS + fish), provided the report states the difference. A careless attempt that happens to name a difference would also pass this check, so it relies on `outcome-honest` and on the good-faith wording to catch it. I accept that miss. The final full run confirmed 20/20 with the same `rubric.md` I uploaded, so nothing else flipped when I loosened this check.
