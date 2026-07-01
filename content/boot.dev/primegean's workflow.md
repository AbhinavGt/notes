# My Workflow

While on the topic of pull requests, I want to share a note on my simple workflow. 90% of the time, you're only using a handful of git commands to get your coding work done.
Keep Stuff on GitHub

I keep all my serious projects on GitHub. That way if my computer explodes, I have a backup, and if I'm ever on another computer, I can just clone the repo and get back to work.

# Rebase vs. Merge

I've configured Git to rebase by default on pull, rather than merge so I keep a linear history. If you want to do the same, you can add this to your global Git config:

`git config set --global pull.rebase true`

# My Solo Workflow

When I'm working by myself, I usually stick to a single branch, main. I mostly use Git on solo projects to keep a backup remotely and to keep a history of my changes. I only rarely use separate branches.

1. Make changes to files
2. `git add . (or git add <files> if I only want to add specific files)`
3. `git commit -m "a message describing the changes"`
4. `git push origin main`

It really is that simple for most solo work. git log, git reset, and some others are, of course, useful from time to time, but the above is the core of what I do day-to-day.

# My Team Workflow

When you're working with a team, Git gets a bit more involved (and we'll cover more of this in part 2 of this course). Here's what I do:

- Update my local main branch with git pull origin main
- Checkout a new branch for the changes I want to make with `git switch -c <branchname>`
- Make changes to files
- `git add .`
- `git commit -m "a message describing the changes"`
- `git push origin <branchname>` (I push to the new branch name, not main)
- Open a pull request on GitHub to merge my changes into main
- Ask a team member to review my pull request
- Once approved, click the "Merge" button on GitHub to merge my changes into main
- Delete my feature branch, and repeat with a new branch for the next set of changes
