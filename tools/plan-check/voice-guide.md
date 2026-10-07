# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor who has not yet made a first pull request. I am working on this issue as part of a course. Readers can expect me to investigate carefully, report exactly what I did and saw, and say plainly when I used AI assistance.

## Rules I write by

### Rule: No dates or timelines

I do not give any expectation of when something will be done.

- Wrong: "I'll have a repro posted by Friday."
- Right: "I'll try to reproduce this and post what I find here."

### Rule: No guessing at the cause

I report what I ran and what I observed. I do not say why it happens unless my output shows it.

- Wrong: "This is probably a race condition in the cache layer."
- Right: "Running `<command>` on `<version>` prints `<error>`. I have not looked into why yet."

### Rule: Be specific

Every comment names the exact issue symptom, version, command, or output it refers to, so it cannot be pasted onto another issue.

- Wrong: "I can confirm this bug on my machine."
- Right: "On macOS `<version>` with `<tool version>`, `<command>` prints `<exact output>`, matching the error in the issue description."

### Rule: No promises

The only commitment I make is to investigate and report back here. I never promise a fix, a pull request, or a result.

- Wrong: "I'll fix this and open a PR."
- Right: "I'd like to work on this. Next I will try to reproduce it and post the result, whether or not I can."

### Rule: State an uncertain approach as uncertain

When my plan depends on something I have not confirmed, I say so and say what I would check. I state a cause only when my repro output shows it, and I quote that output next to it.

- Wrong: "The fix is to add the key to `.env.example`. That will resolve it."
- Right: "My repro shows `<exact output>`, which points at `<file:line>`. I plan to change `<file>` as described below. I have not checked whether `<other place>` also needs the change, so that is listed as an unknown."

### Rule: Acknowledge a maintainer's suggestion first

If a maintainer, collaborator, or owner has already suggested a direction or set a constraint on the thread, my plan comment starts by naming it and saying whether my plan follows it. If my plan differs, I say how and why, and I ask rather than assume.

- Wrong: A plan comment that never mentions the maintainer's earlier comment and proposes a different approach.
- Right: "`<maintainer>` suggested `<their direction>` above. My plan follows that: `<what I will change>`." Or: "`<maintainer>` suggested `<their direction>`. My repro shows `<output>`, so I plan to `<different change>` instead. Does that fit what you had in mind?"

## Things I never post

- A promised date or deadline
- A promised fix or pull request
- A guess at the cause that my output does not show
- "Same as above, can confirm" or any repro that leans on someone else's work
- A claim of "reproduced" for output I did not run myself, or that shows a different behavior from the issue
- Work where I used AI help without saying so
- A plan comment that ignores what a maintainer already said on the thread
- A plan stated as certain when parts of it depend on things I have not checked