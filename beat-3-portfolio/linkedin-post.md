I made my First Attempt In Learning an open source contribution. My pull request was closed, but the experience gave me so many areas to explore.

Before my pull request could go out, it had to pass a checker I set up. The checker ran the project's own tests and style checks, then graded my change against my written plan. It stopped me twice: once because some of the project's tests already fail on Windows, and once because it saw changes in my branch that weren't mine. Both times I fixed my setup instead of skipping the check, and the pull request went out with all six of the project's automated checks passing.

The maintainer replied within two hours. The issue wasn't a priority for the project, and my approach wasn't what he wanted. I thanked him and closed the pull request myself.

What I'm taking away:
- Communicate with the maintainers and ask what they would find useful before working on an issue.
- Automated checks are worth the friction when they catch real mistakes.
- Lastly, I gained confidence in myself that I can use for my future contributions.

https://github.com/scikit-hep/pyhf/pull/2800