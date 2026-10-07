# Plan: issue #73, `.env.example` is missing the `OPENROUTER_API_KEY` that `README.md` tells readers to set

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863156497
Checked at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779` on `main` of my fork (`adarsh1343/pathreview-ai301-fa26-s1`).

## Diagnosis

The template file that the README's copy step produces has no line for the key the README tells the reader to set. My repro output shows:

- `README.md:24`: `# Configure environment (add your OPENROUTER_API_KEY to .env)`, and `README.md:25`: `cp .env.example .env`
- `.env.example` contains only `LLM_PROVIDER=mock` and `OPENAI_API_KEY=sk-your-key-here`. `Select-String -Path .env.example -Pattern "OPENROUTER"` printed nothing.
- `core\config.py:20`: `openrouter_api_key: str = Field(default="")`. Lines 21 and 22 define `openrouter_base_url` and `openrouter_model` with defaults.
- After `Copy-Item .env.example .env`, `Select-String -Path .env -Pattern "OPENROUTER"` printed nothing, so the `.env` the README's copy step creates has no `OPENROUTER_API_KEY` entry.

So the missing line is in `.env.example`. `README.md` and `core/config.py` already refer to the key. I have not looked at why the line is missing, and I did not run the app.

## Scope

In scope:
- Add an `OPENROUTER_API_KEY` line to `.env.example`.
- Update the `LLM_PROVIDER` comment in `.env.example` to list `openrouter`, only if step 1 of the approach confirms the code accepts that value.

Not in scope:
- Any change to `README.md`. It already names the key, and the repro does not show it as wrong.
- Any change to `core/config.py` or other code.
- Adding `OPENROUTER_BASE_URL` or `OPENROUTER_MODEL` to `.env.example`. They have defaults in `core/config.py:21-22`, and the issue is about the API key.
- Other lint, type, or docs clean-up. CONTRIBUTING.md asks for those as separate PRs.

## Files I'll touch

- `.env.example` (the only file I expect to change)

## Approach

In this order:
1. Find where `llm_provider` is used in the code (for example `git grep -n "llm_provider"`) and note which values it accepts. This answers the question my repro left open.
2. In `.env.example`, add `OPENROUTER_API_KEY=your-openrouter-key-here` on the line after `OPENAI_API_KEY=sk-your-key-here`.
3. If step 1 shows `openrouter` is an accepted `LLM_PROVIDER` value, change the comment `# Options: "mock" (default, no API key needed), "openai"` to also list `"openrouter"`. If step 1 shows it is not accepted or is unclear, leave the comment alone and say so in the PR and in `## Deviations`.
4. Re-run the repro steps (test plan) and `make check && make test-unit`.

Branch: `docs/73-add-openrouter-key-to-env-example`, from `main`, per CONTRIBUTING.md's `<type>/<issue-number>-<short-description>` format.

## Test plan

My unit 2 repro steps, re-run after the change, from the repo root in PowerShell:

1. `Select-String -Path .env.example -Pattern "OPENROUTER"`
   - Before the fix: no output.
   - Expected after: one match, the line `OPENROUTER_API_KEY=your-openrouter-key-here`.
2. `Copy-Item .env.example .env`, then `Select-String -Path .env -Pattern "OPENROUTER"`
   - Before the fix: no output.
   - Expected after: one match, the same `OPENROUTER_API_KEY=...` line.
3. `Get-Content .env.example`
   - Expected after: the `LLM_PROVIDER` comment lists `openrouter` if step 3 of the approach applied, and otherwise still lists only `mock` and `openai`.

Then `make check && make test-unit`, as CONTRIBUTING.md asks, and I will report the result. `git grep -n "issue #73"` returns nothing, so there is no xfail marker to remove. I am not adding an automated test, because the change is a line in a template file and not code, and the repro steps above show the before and after.

## Risks and unknowns

- I have not checked which `LLM_PROVIDER` values the code accepts. Step 1 of the approach does that, and it decides whether the comment changes.
- I have not checked that the code reads `OPENROUTER_API_KEY` from `.env` into `openrouter_api_key`. I only saw the field in `core/config.py:20`.
- The placeholder text `your-openrouter-key-here` is my choice. A maintainer may prefer another format, such as one matching the existing `sk-your-key-here`.
- CONTRIBUTING.md lists commit scopes `ingestion`, `rag`, `agent`, `safety`, `api`, `frontend`. None obviously fits a config template, so I am unsure which scope to use in the commit message.
- My repro ran on Windows with PowerShell. I have not checked whether `make` is available there. If it is not, I will say which checks I could not run.

## Deviations

(Fill in after the build.)
