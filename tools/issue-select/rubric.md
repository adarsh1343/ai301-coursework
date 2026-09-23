
# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo-facts block or the last 10 commits on the default branch, with author and timestamp | At least one commit or merged PR from a maintainer/core contributor within the last 60 days | required |
| Repo in use | Repo-facts block (release history, stars) or count of issues/PRs closed in the last 90 days | A release was published in the last 12 months, OR at least 2 issues/PRs were closed in the last 90 days | required |
| Scope fits a newcomer | Issue labels and the issue body text | Issue carries a beginner-oriented label (e.g. "good first issue", "help wanted", "beginner", "easy"), OR the body scopes the change to a single named file/function/component and requires no architecture-level decisions | preferred |
| Nobody else already on it | Issue's assignee field, linked PRs, and the comment thread | No open PR references the issue; the assignee field is empty (or the assignee's last activity on the issue is 30+ days old with no linked commits); no comment claims the issue is already being worked | required |
| Comments replied to in the last 30 days | Timestamps in the issue comment thread | Every comment from a non-maintainer asking to work on the issue, or asking a clarifying question, received a maintainer reply within 30 days of being posted. Passes trivially if no such comment exists | required |
| Familiar issue | Issue body and labels for language/framework/tags | Issue's primary language or framework matches a stack you've listed as familiar | preferred |

## Verdict rule

Accept if and only if all five `required` checks pass. Any `required` check graded `unclear` counts as a fail (fails closed — the default is reject, not accept). The `preferred` check ("familiar issue") never changes the accept/reject verdict; it only ranks accepted issues against each other, with a "pass" ranked above a "fail"/"unclear" on that check.
