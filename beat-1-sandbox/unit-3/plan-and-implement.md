# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

adarsh1343

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-6030359140

Plan for this issue, based on my repro in [this comment](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863156497) (commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`).

**What my repro shows:** `README.md:24` says `# Configure environment (add your OPENROUTER_API_KEY to .env)` and line 25 runs `cp .env.example .env`. `.env.example` contains only `LLM_PROVIDER=mock` and `OPENAI_API_KEY=sk-your-key-here`, and `Select-String -Path .env.example -Pattern "OPENROUTER"` printed nothing. `core\config.py:20` defines `openrouter_api_key`. After `Copy-Item .env.example .env`, the same search on `.env` also printed nothing. I did not run the app.

**Change I plan to make:** in `.env.example`, add an `OPENROUTER_API_KEY=your-openrouter-key-here` line after `OPENAI_API_KEY`. If the code accepts `openrouter` as an `LLM_PROVIDER` value, I would also add it to the `Options:` comment there. I have not checked that yet, so I will look at where `llm_provider` is used first, and leave the comment alone if it is not accepted or unclear.

**Not changing:** `README.md` (it already names the key), `core/config.py` or any other code, and `OPENROUTER_BASE_URL` / `OPENROUTER_MODEL` (they have defaults in `core/config.py:21-22`).

**Files:** `.env.example` only. Branch name would be `docs/73-add-openrouter-key-to-env-example`.

**How I would check it:** re-run my repro steps. `Select-String -Path .env.example -Pattern "OPENROUTER"` should print the new `OPENROUTER_API_KEY` line instead of nothing, and after `Copy-Item .env.example .env` the same search on `.env` should print it too. I would also run `make check && make test-unit` as CONTRIBUTING.md asks. `git grep -n "issue #73"` finds no xfail marker, and since this is a template line and not code, I am not planning a new automated test.

**Unknowns:**
- Which `LLM_PROVIDER` values the code accepts (see above).
- Whether `OPENROUTER_API_KEY` in `.env` is read into `openrouter_api_key`. I have only seen the field in `core/config.py`.
- The placeholder text is my choice. I am happy to match whatever format you prefer.
- None of the commit scopes in CONTRIBUTING.md obviously fits a config template, so I am unsure which one to use.

I used AI assistance (Claude) while preparing this plan.

---

## Your branch

**Branch**

docs/73-add-openrouter-key-to-env-example

**Evidence**

Before (from my repro comment, https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863156497, at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`):

```
PS> Select-String -Path .env.example -Pattern "OPENROUTER"
PS>

PS> Copy-Item .env.example .env
PS> Select-String -Path .env -Pattern "OPENROUTER"
PS>
```

After (on branch `docs/73-add-openrouter-key-to-env-example`):

```
PS C:\Users\Adarsh\Downloads\codepath\pathreview-ai301-fa26-s1> git diff
diff --git a/.env.example b/.env.example
index 1be8b38..a0d1750 100644
--- a/.env.example
+++ b/.env.example
@@ -17,6 +17,7 @@ VECTOR_DB_URL=http://localhost:8001
 # Options: "mock" (default, no API key needed), "openai"
 LLM_PROVIDER=mock
 OPENAI_API_KEY=sk-your-key-here
+OPENROUTER_API_KEY=your-openrouter-key-here
 
 # App settings
 APP_ENV=development

PS> Select-String -Path .env.example -Pattern "OPENROUTER"

.env.example:20:OPENROUTER_API_KEY=your-openrouter-key-here

PS> Copy-Item .env.example .env
PS> Select-String -Path .env -Pattern "OPENROUTER"

.env:20:OPENROUTER_API_KEY=your-openrouter-key-here
```

Repo checks: `make` was not installed in my PowerShell at first (`make : The term 'make' is not recognized as the name of a cmdlet, function, script file, or operable program.`). I installed GnuWin32 Make, created the project's `.venv`, and ran the Makefile targets one at a time. I did not run `make check`, because its `format` step runs `black .` and rewrites files, and this change only touches `.env.example`.

```
PS> make lint
.venv/Scripts/ruff check .
All checks passed!

PS> make typecheck
.venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files

PS> make test-unit
.venv/Scripts/pytest tests/unit -v -m unit
collected 428 items
...
375 passed, 53 xfailed, 2 warnings in 37.81s
```

The 53 xfailed tests are existing tests that are marked as expected failures for other issues, and none of them is for #73 (`git grep -n "issue #73"` returns nothing). The diff changes only `.env.example`.

---

## Eval iterations

**Run history**

19/20, then 18/20

**Package analysis**

Package: pkg-09 (sharkdp/fd#2067).

My rubric decided: accept. The gold label said: accept. They agreed.

Why my rubric read it that way:

- Diagnosis matches repro: the plan says the glob-derived regex "expects `/` separators but is matched against raw native paths with `\`", and the repro evidence says "confirming the candidates carry backslashes at match time", with the regex-mode control matching `C:\t\fixture\src\foo\a.spec.ts`. The cause is consistent with the evidence.
- Fix targets the cause: the change maps `\` to `/` in the match candidate, so it removes the mismatch between separators instead of working around the output.
- Thread-aligned: collaborator tmccombs lists three options, and the plan says "option 2 from the thread, which the collaborator noted is the simpler route", and states why option 1 is deferred. A maintainer signal exists, and the plan follows it.
- Repo conventions followed: the repo facts say "the policy states no disclosure ask for issue comments", so the comment needs no AI-use disclosure. A rubric that demanded one here would reject a ready plan. My check passes when every ask the repo states is met, and this comment meets them.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

> | Thread-aligned | The plan comment and the plan, read against the thread highlights and issue context (maintainer, collaborator, or repo-owner comments). | If a maintainer, collaborator, or owner has stated a direction, constraint, or objection, the plan accounts for it: it follows it, or says explicitly why it differs. Automatic pass if the thread has no maintainer, collaborator, or owner comments. Fail if such a comment exists and the plan or comment ignores or contradicts it. | required |

Why it reads that way: in the group activity, grading calib-03 with the sample rubric gave `ready`, but the correct verdict was `hold`, because the candidate "did not catch collaborator comment", and the sample rubric had no check that looked at the thread at all. I added this check, and made it required because ignoring a maintainer's direction is a reason to hold a plan. When we graded calib-01, which has no maintainer comments, the check was unclear about what to do with an empty thread, so I added the sentence "Automatic pass if the thread has no maintainer, collaborator, or owner comments." I rejected a version that asked only whether the plan "takes comments into account", because it does not say whose comments count or what passing looks like, and two graders would read it differently.

**Trade-offs**

This check gives up a case: it only requires the plan to account for comments from a maintainer, collaborator, or owner. In pkg-09, petrroll (NONE) posted a competing implementation in PR #2089. My check does not require the plan to answer that, because a non-maintainer's comment is not a signal the rubric counts. The plan comment in pkg-09 does mention #2089, but a plan that ignored it would still pass this check. I accept that gap because treating every commenter's suggestion as binding would hold plans for ignoring drive-by comments, and the check would no longer track what maintainers asked for.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.
