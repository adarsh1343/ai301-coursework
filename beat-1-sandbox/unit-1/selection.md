# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

"https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73"

**Verdict output**
{
   "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
   "checks": [
   {"name": "Maintainer alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 7 days before grading"},
   {"name": "Repo in use", "grade": "pass", "evidence": "2 issues closed since 2026-06-25 (#52 and #43, both 2026-09-16), meeting the threshold of at least 2 in 90 days"},
   {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Labeled 'good first issue' + 'docs'; body scopes the change to README.md and .env.example"},
   {"name": "Nobody else already on it", "grade": "pass", "evidence": "assignees empty, 0 comments, no linked or cross-referenced PRs in the timeline"},
   {"name": "Comments replied to in the last 30 days", "grade": "pass", "evidence": "Passes trivially: comment count is 0"},
   {"name": "Familiar issue", "grade": "unclear", "evidence": "scope.md fit profile is still the placeholder '(Write a few sentences here.)' — no familiar stack declared"}
   ],
   "verdict": "accept"
},

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

9/20
13/20
18/20

**Issue analysis**

grading 1 bundle(s) with rubric.md, model sonnet, 5 worker(s)...
  issue-04: accept

item      gold    verdict  agree  note
issue-04  accept  accept   yes

agreement: 1/1 scored items

reasoning: passes all required checks in rubric.md (Maintainer alive, Repo in use, Nobody else already on it, Comments replied to in the last 30 days)

**Check rationale**

check: | Comments replied to in the last 30 days | Timestamps in the issue comment thread | Every comment from a non-maintainer asking to work on the issue, or asking a clarifying question, received a maintainer reply within 30 days of being posted. Passes trivially if no such comment exists | required |

reason: proves that the maintainers are not the only ones commenting and also that the maintainers are responding on time.

**Trade-offs**

gives up detecting maintainer responsiveness on issues with zero comments. it passes trivially rather than actually testing anything. A newcomer relying on this check alone for a silent issue is really only protected by the separate "Maintainer alive" check, not by this one. I accept that trade-off because requiring engagement history on every issue would reject perfectly good silent-but-fresh issues, which would hurt more than it protects.

---

## Selection rationale

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

1. limited time this week, interest in keys and understanding how they work
2. the verdict correctly identified if the issue had 2 issues closed since 2026-06-25 (#52 and #43, both 2026-09-16), meeting the threshold of at least 2 in 90 days. I weighed the fact that the issue would only take 1-2 hours, which makes it a beginner-friendly issue.
3. understanding where to find the relevant files, 