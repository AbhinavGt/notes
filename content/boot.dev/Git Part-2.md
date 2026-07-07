Important commands
- `git log --oneline -1 -p`
	- Shows the contents of the previous commit
- `git commit --amend`
	- This command gives you change to change the last commit message
	- But it changes your commit hash. Thus this is a destructive history altering command
- `git reset --soft HEAD~1`
	- To undo the last commit completely
	- Since the changes are soft it leaves changes in index and worktree.
	- Used often in rebase and merge conflict
- `git reset --hard HEAD~1`
	- Not only it un-does the commit it deletes the file as well
# Fork
- A fork is a copy of a repository.
- Forking a repository allows you to freely experiment with changes without affecting the original project.
- Forking is not a Git operation, but it is a feature offered by many Git hosting services such as GitHub, GitLab, and Bitbucket.
- Those services "fork" a repo by creating a new copy of the repo and associating it as a "fork" of the original. It's quite literally just a copy that is linked to the original via some metadata.
## Disable RERERE

```bash
➜  megacorp git:(main) git config set --local rerere.enabled false
➜  megacorp git:(main) rm -rf .git/rr-cache
```

# PRs from a Fork
- When you fork someone's repository on a platform like GitHub, you get a copy of the repository in your account.
- This is the standard way to contribute to someone else's open-source project.
- The steps are typically:
	1. Fork their repo (main repo) into your account
	2. Clone down your fork to your local machine (environment)
	3. Create a new branch (let's call it `your_feature`). This is your main branch in forked repo
	4. Make changes to the main branch
	5. Commit and push changes to your fork's remote `your_feature` branch in the forked repo
	6. Create a pull request to `original_owner/repo` main from `your_username/repo your_feature` (ie PR with changes from the fork onto the original repo)
- Then the original owner can review your changes.
- If they like them, they can merge the changes straight from your fork into their repository

> [!note] Keep in mind 
 > - Also whenever you clone our forked repo.
 > - Add a second remote called upstream
> 	- `git remote add upstream https://github.com/ORIGINAL-OWNER/ORIGINAL-REPO.git`
> - Whenever the original repo changes. 
> - You can bring it that changes and the way when you fix something. You know that you are fixing on the latest version; Not accedentaly fixing something someone else already fixed
> - This also avoides having conflicts during PR's 

 

- Why fork? If you are interested in contributing to the main 
- If you are not interested and just poking around. You clone!

```bash
➜  megacorp git:(main) git switch -c "add_contrib"
Switched to a new branch 'add_contrib'
➜  megacorp git:(add_contrib) nvim .
➜  megacorp git:(add_contrib) ✗ git add .
➜  megacorp git:(add_contrib) ✗ git commit -m "add myself as a contributor"
[add_contrib 5918b32] add myself as a contributor
 1 file changed, 1 insertion(+)
 create mode 100644 contributors/AbinavGt.txt
➜  megacorp git:(add_contrib) git push origin add_contrib
fatal: unable to access 'https://github.com/AbhinavGt/megacorp/': Could not resolve host: github.com
➜  megacorp git:(add_contrib) git push origin add_contrib
Username for 'https://github.com': AbhinavGt
Password for 'https://AbhinavGt@github.com':
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 423 bytes | 423.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote:
remote: Create a pull request for 'add_contrib' on GitHub by visiting:
remote:      https://github.com/AbhinavGt/megacorp/pull/new/add_contrib
remote:
To https://github.com/AbhinavGt/megacorp
 * [new branch]      add_contrib -> add_contrib

➜  megacorp git:(add_contrib) git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
➜  megacorp git:(main) git branch -D add_contrib
Deleted branch add_contrib (was 5918b32).
```

![[pullReq-megacorp.png]]
## The Workflow

# Reflog
## HEAD
- You'll see a big scary word in the Git world called `HEAD`.
- Branches are references to commits, and `HEAD` is a reference to the branch you're currently on.

```bash
➜  megacorp git:(main) cat .git/HEAD
ref: refs/heads/main
➜  megacorp git:(main) git log

commit 0d16f954dfda53be7e875da6a23698c428cf68af (HEAD -> main, origin/main, origin/HEAD)
Author: wagslane <lane.c.wagner@gmail.com>
Date:   Wed Jun 26 11:22:51 2024 -0600

    A: The Founding of MegaCorp and the End of Art
```

- The git reflog command is kinda like git log but stands for "reference log" and specifically logs the changes to a "reference" that have happened over time.\
- Reflog uses a different format to show the history of a branch or HEAD: one that's more concerned with the number of steps back in time. For example:

| reflog   | meaning                    |
| -------- | -------------------------- |
| HEAD@{0} | where HEAD is now          |
| HEAD@{1} | where HEAD was 1 move ago  |
| HEAD@{2} | where HEAD was 2 moves ago |
| HEAD@{3} | where HEAD was 3 moves ago |
| ...      | ...                        |

```bash
➜  megacorp git:(main) git reflog
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{0}: checkout: moving from add_contrib to main
5918b32 (origin/add_contrib) HEAD@{1}: commit: add myself as a contributor
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{2}: checkout: moving from main to add_contrib
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{3}: clone: from https://github.com/AbhinavGt/megacorp

➜  megacorp git:(main) git switch -c slander
Switched to a new brranch 'slander'

➜  megacorp git:(main) git reflog
0d16f95 (HEAD -> slander, origin/main, origin/HEAD, main) HEAD@{0}: checkout: moving from main to slander
0d16f95 (HEAD -> slander, origin/main, origin/HEAD, main) HEAD@{1}: checkout: moving from add_contrib to main
5918b32 (origin/add_contrib) HEAD@{2}: commit: add myself as a contributor
0d16f95 (HEAD -> slander, origin/main, origin/HEAD, main) HEAD@{3}: checkout: moving from main to add_contrib
0d16f95 (HEAD -> slander, origin/main, origin/HEAD, main) HEAD@{4}: clone: from https://github.com/AbhinavGt/megacorp

➜  megacorp git:(slander) ✗ git add .
➜  megacorp git:(slander) ✗ git commit -m "B: slander"
[slander 2ab2d17] B: slander
 1 file changed, 4 insertions(+)

➜  megacorp git:(main) git switch -c slander
create mode 100644 slander.md
2ab2d17 (HEAD -> slander) HEAD@{0}: commit: B: slander
0d16f95 (origin/main, origin/HEAD, main) HEAD@{1}: checkout: moving from main to slander
0d16f95 (origin/main, origin/HEAD, main) HEAD@{2}: checkout: moving from add_contrib to main
5918b32 (origin/add_contrib) HEAD@{3}: commit: add myself as a contributor
0d16f95 (origin/main, origin/HEAD, main) HEAD@{4}: checkout: moving from main to add_contrib
0d16f95 (origin/main, origin/HEAD, main) HEAD@{5}: clone: from https://github.com/AbhinavGt/megacorp
```

# Delete and Recover Branch

```bash
➜  megacorp git:(slander) git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
➜  megacorp git:(main) git branch -D slander
Deleted branch slander (was 2ab2d17)

➜  megacorp git:(main) git reflog
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{0}: checkout: moving from slander to main
2ab2d17 HEAD@{1}: commit: B: slander
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{2}: checkout: moving from main to slander
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{3}: checkout: moving from add_contrib to main
5918b32 (origin/add_contrib) HEAD@{4}: commit: add myself as a contributor
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{5}: checkout: moving from main to add_contrib
0d16f95 (HEAD -> main, origin/main, origin/HEAD) HEAD@{6}: clone: from https://github.com/AbhinavGt/megacorp
(END)

➜  megacorp git:(main)  git cat-file -p 2ab2d17
tree e61f86bc41817b68b378a55cb6f62c8357e6d082
parent 0d16f954dfda53be7e875da6a23698c428cf68af
author Victon18 <abhinavbatham333@gmail.com> 1782971350 +0530
committer Victon18 <abhinavbatham333@gmail.com> 1782971350 +0530

B: slander

➜  megacorp git:(main) git cat-file -p e61f86bc41817b68b378a55cb6f62c8357e6d082
100644 blob 13b4fae17d1b50828697a36c4ab337039018a35e    README.md
040000 tree 929e037cad8c9a63e690b3ed15ef05db3feb04ff    contributors
040000 tree f408a5791812c598240e2e5e30759675a33957e8    customers
040000 tree 3f24c2dac3af70dc3af6713f686d4f57d9608a42    orgs
040000 tree 87e79e11ffc811016818fe57ab91fac55cb5a8b8    scripts
100644 blob 30fbe30f2546d07d072707dc4fc46079ee26ff09    slander.md
➜  megacorp git:(main) git cat-file -p 30fbe30f2546d07d072707dc4fc46079ee26ff09
# Breaking News

MegaCorp CEO Lane enjoyed the live-action Last Airbender movie
MegaCorp CTO ThePrimeagen is a fan of The Notebook (and most other Nicholas Sparks content)

➜  megacorp git:(main) ✗ git add slander.md
➜  megacorp git:(main) ✗ git commit -m "B: recovery"
[main 20b7194] B: recovery
 1 file changed, 4 insertions(+)
 create mode 100644 slander.md
```

## Merge
- Using Git internals is exceptionally inconvenient.
- The `git merge` command actually takes a "commitish" as an argument:
	- `git merge <commitish>`
- A "commitish" is something that looks like a commit (branch, tag, commit, `HEAD@{1}`)
- In other words instead of:

```bash
git reflog # find the commit sha at HEAD@{1}
git cat-file -p <commit sha>
git cat-file -p <tree sha>
git cat-file -p <blob sha> > slander.md
git add .
git commit -m "B: recovery"
```

- We could have just done:
	- `git merge HEAD@{1}`
# Merge Conflict
## Conflicting Changes
- Things get a little hairy when changes to the same lines are made at the same time (e.g. one commit isn't the parent of another).

```bash
➜  megacorp git:(main) git switch -c add_customers
Switched to a new branch 'add_customers'
➜  megacorp git:(add_customers) ✗ git add .
➜  megacorp git:(add_customers) ✗ git commit -m "C: update customers and partners"
[add_customers 9fcf449] C: update customers and partners
 2 files changed, 3 insertions(+)
➜  megacorp git:(add_customers) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) nvim .
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "D: add jayson to customers"
[main 119e1d9] D: add jayson to customers
 1 file changed, 1 insertion(+)

```
## Merge Conflict
- A merge conflict occurs when two commits modify the same line and Git can't automatically decide which change to keep and which change to discard.
- When a conflict happens (usually as the result of a merge or rebase) Git will prompt you to manually decide which change to keep. 
- It's okay when the same line is modified in one commit, and then again in a later commit. 
- The problem arises when the same line is modified in two commits that aren't in a parent-child relationship.

### Example
- Consider the following commit history:
```
    C     feature
  /
A - B     main
```
- The `main` branch has a file with these lines:
```python
package main

func isNice(num int) bool {
    return num == 69 // commit B changed this line
}
```
- While feature has:
```python
package main

func isNice(num int) bool {
    return num == 420 // commit C changed this line
}
```
- If we merge `feature` into `main`, Git will detect that the return line was changed in both branches independently: which creates a conflict.
## Merge
- Conflicting changes on two different branches is not a problem. 
- The problem only arises when you try to merge those branches. 
- When you do, Git will detect the conflict and ask you to resolve it

```bash
➜  megacorp git:(main) git switch add_customers
Switched to branch 'add_customers'
➜  megacorp git:(add_customers) git merge main
Auto-merging customers/all.csv
CONFLICT (content): Merge conflict in customers/all.csv
Automatic merge failed; fix conflicts and then commit the result.
➜  megacorp git:(add_customers) ✗ git status
On branch add_customers
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   customers/all.csv

no changes added to commit (use "git add" and/or "git commit -a")
```
##  Edit the File
- Resolving conflicts is a manual process. 
- When they happen, Git marks the conflicted files and asks you to resolve the conflict by editing the files in your editor.
```
➜  megacorp git:(add_customers) ✗ cat customers/all.csv
first_name,last_name,company,title
<<<<<<< HEAD
karson,yummy,intercooler,ceo
=======
jayson,gross,htmz,contributor
>>>>>>> main
```

- The top section, between the `<<<<<<< HEAD` and `=======` lines, is our branch's version of the file. 
- The bottom section, between the `=======` and `>>>>>>> main` lines, is the version of the file that's on the main branch ("theirs" or as I say "Stupid Greg's").
- In many cases, you might want to keep one change and discard the other.
- That's common when you're dealing with code changes. 
- In this case, we're dealing with content, so we want to keep both changes.
- Hence we delete the conflict markers and leave both your change and Greg's change in the file.

```bash
➜  megacorp git:(add_customers) ✗ cat customers/all.csv
first_name,last_name,company,title
karson,yummy,intercooler,ceo
jayson,gross,htmz,contributor
```
## Resolution
- After manually editing any conflicting files (sometimes it's more than one file or more than one section of a file) you need to simply add and commit the changes.
- This tells Git that you've resolved the conflict and it can continue with the merge

```
➜  megacorp git:(add_customers) ✗ git add .
➜  megacorp git:(add_customers) ✗ git commit -m "E: resolve conflict with stupid Greg's customer"
[add_customers 876b017] E: resolve conflict with stupid Greg's customer
➜  megacorp git:(add_customers) git log --oneline

876b017 (HEAD -> add_customers) E: resolve conflict with stupid Greg's customer
119e1d9 (main) D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Ar
```

- Doing a reset --hard of the merge commit, allow us to take a different approach to resolving the conflict.
```bash
➜  megacorp git:(add_customers) git reset --hard 9fcf4493d645b539165fff5deeab7f102823a70d

➜  megacorp git:(add_customers) git log --oneline

9fcf449 (HEAD -> add_customers) C: update customers and partners
20b7194 B: recovery
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Art

➜  megacorp git:(add_customers) git merge main
Auto-merging customers/all.csv
CONFLICT (content): Merge conflict in customers/all.csv
Automatic merge failed; fix conflicts and then commit the result.
➜  megacorp git:(add_customers) ✗ cat customers/all.csv
first_name,last_name,company,title
karson,yummy,intercooler,ceo
➜  megacorp git:(add_customers) ✗ git add customers/all.csv
➜  megacorp git:(add_customers) git commit -m "E: fine, Greg can have this one"
[add_customers 0ef8127] E: fine, Greg can have this one
➜  megacorp git:(add_customers) git log --oneline

0ef8127 (HEAD -> add_customers) E: fine, Greg can have this one
119e1d9 (main) D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Art
```

## Ours and theirs
- In a merge conflict:
    - "Ours" refers to the branch you are on (merging into)
    - "Theirs" refers to the branch being merged
-  Git has a built-in merge resolution tool.

Current commit history
```
A - B - D       main
     \   \
      C - E   add_customers
```

```bash
➜  megacorp git:(main) git merge add_customers
Updating 119e1d9..0ef8127
Fast-forward
 customers/all.csv | 2 +-
 orgs/partners.txt | 2 ++
 2 files changed, 3 insertions(+), 1 deletion(-)
➜  megacorp git:(main) cat customers/all.csv
first_name,last_name,company,title
karson,yummy,intercooler,ceo
➜  megacorp git:(main) git branch -d add_customers
Deleted branch add_customers (was 0ef8127).
```

## Multi-conflict
- We also have to deal with multiple conflict
```bash
➜  megacorp git:(main) git switch -c delete_records
Switched to a new branch 'delete_records'
➜  megacorp git:(delete_records) nvim ,
➜  megacorp git:(delete_records) ✗ git add .
➜  megacorp git:(delete_records) ✗ git commit -m "F: Multi-conflict del_rec"
[delete_records ac96e11] F: Multi-conflict del_rec
 2 files changed, 1 insertion(+), 2 deletions(-)
➜  megacorp git:(delete_records) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 4 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) nvim
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "G: Multi-conflict main"
[main f9e59a1] G: Multi-conflict main 
 2 files changed, 2 insertions(+), 2 deletions(-)
```

Git history
```
                  F   delete_records
                 /
A - B - C - D - E - G   main
```

## Checkout Conflict
- The git checkout command can checkout the individual changes during a merge conflict using the `--theirs` or `--ours` flags.
	- `--ours` keeps the version of the file from your current branch (the one you're on before merging)
	- `--theirs` uses the version of the file from the branch you're merging into your current branch
- `git checkout --theirs path/to/file`

```bash
➜  megacorp git:(main) git merge delete_records
Auto-merging customers/all.csv
CONFLICT (content): Merge conflict in customers/all.csv
Auto-merging orgs/partners.txt
CONFLICT (content): Merge conflict in orgs/partners.txt
Automatic merge failed; fix conflicts and then commit the result.
➜  megacorp git:(main) ✗ git checkout --theirs customers/all.csv
Updated 1 path from the index
➜  megacorp git:(main) ✗ git checkout --ours orgs/partners.txt
Updated 1 path from the index
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "H: Resolved multi-conflict"
[main f760d04] H: Resolved multi-conflict
➜  megacorp git:(main) git branch -d delete_records
Deleted branch delete_records (was 58cef71).
➜  megacorp git:(main) git log --oneline

f760d04 (HEAD -> main) H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Art
```

- When resolving merge conflicts sometimes you don't get the "standard" merge commit message:
	- Merge branch 'main' into add_customers
- You have to manually provide a message.
# Rebase Conflicts
-  A lot of rebase's bad rep comes from conflicts
- Rebasing feels scarier because it rewrites Git history, which means if you're not careful, you can lose work in an unrecoverable way.
- But as long as you understand what's going on, it will make your (and your team's) Git history cleaner and easier to understand.

## Make the conflict

- In the "real world," what happens most often is:
	- You switch to a new branch, say `fix_bug`, which is a copy of main.
	- While you're fixing the bug, someone else merges their changes into main.
	- You fix the bug, and it so happens that you edited the same files (and lines) that the other person did.
	- You open a Pull Request to merge (or rebase) `fix_bug` into `main`, then Git tells you there's a conflict.
	- You resolve the conflict on your branch.
	- You complete the Pull Request with the conflict resolved.
```bash
➜  megacorp git:(main) git switch -c banned
Switched to a new branch 'banned'
➜  megacorp git:(banned) ✗ git add customers/banned.csv
➜  megacorp git:(banned) ✗ git commit -m "I: Banned.csv from banned"
[banned 128d8e8] I: Banned.csv from banned
 1 file changed, 3 insertions(+)
 create mode 100644 customers/banned.csv

➜  megacorp git:(banned) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 7 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) ✗ git add customers/banned.csv
➜  megacorp git:(main) ✗ git commit -m "J: Banned.csv from main"
[main 39b08d7] J: Banned.csv from main
 1 file changed, 3 insertions(+)
 create mode 100644 customers/banned.csv
 ➜  megacorp git:(main) git switch banned
Switched to branch 'banned'

➜  megacorp git:(banned) git rebase main
Auto-merging customers/banned.csv
CONFLICT (add/add): Merge conflict in customers/banned.csv
error: could not apply 128d8e8... I: Banned.csv from banned
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config set advice.mergeConflict false"
Could not apply 128d8e8... # I: Banned.csv from banned
➜  megacorp git:(39b08d7) ✗ git log --all --oneline
39b08d7 (HEAD, main) J: Banned.csv from main
128d8e8 (banned) I: Banned.csv from banned
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
5918b32 (origin/add_contrib) add myself as a contributor
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Art
```

# Resolving the conflict
- The same git checkout `--theirs` and git checkout `--ours` commands we used with merge can be used to resolve conflicts during a rebase
> [!note] Note 
> In a merge, `--ours` refers to your current branch, but in a rebase, `--ours` refers to the branch you're rebasing onto.
- With `rebase` conflicts, unlike merge conflicts, we don't `commit` to resolve the conflict. Instead, we `--continue` the rebase

```bash
➜  megacorp git:(39b08d7) ✗ git checkout --ours customers/banned.csv
Updated 1 path from the index
➜  megacorp git:(39b08d7) ✗ git add .
➜  megacorp git:(39b08d7) git rebase --continue
Successfully rebased and updated refs/heads/banned.
➜  megacorp git:(banned) git log --oneline

39b08d7 (HEAD -> banned, main) J: Banned.csv from main
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 (origin/main, origin/HEAD) A: The Founding of MegaCorp and the End of Art

➜  megacorp git:(banned) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 8 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) git branch -d banned
Deleted branch banned (was 39b08d7).
```

## Deleted Commit

- Why is our `I:` commit just gone!?!
- Well, let's think about what happened during our rebase conflict resolution:
	- We started a rebase of `banned` onto `main`, meaning we're rewriting the history of `banned` to include all the changes from `main`.
	- We effectively removed all the changes from the `I` commit by choosing to keep the changes from `main` instead of `banned`.
	- We continued the rebase, and Git realized that the `I` commit was pointless, and because we're rewriting history anyway, it just removed it.
- If our changes had been more complicated, say we had kept some of the changes from the I commit, and overwritten others, then Git would have kept the `I` commit in the history.
## Repeat Resolution Step
- A common complaint about rebase is that there are times when you may have to manually resolve the same conflicts over and over again.
- This is especially prevalent when you have a long-running feature branch, or even more likely, multiple feature branches that are being rebased onto `main`.
### RERERE to the Rescue

- The `git rerere` functionality is a bit of a hidden feature. 
- The name stands for "reuse recorded resolution" and, as the name implies, it allows you to ask Git to remember how you've resolved a hunk conflict so that the next time it sees the same conflict, Git can resolve it for you automatically.
- In other words, if enabled, `rerere` will remember how you resolved a conflict (applies to rebasing but also merging) and will automatically apply that resolution the next time it sees the same conflict.
```bash
➜  megacorp git:(main) git switch -c favs
Switched to a new branch 'favs'
➜  megacorp git:(favs) ✗ git add customers/favs.md

➜  megacorp git:(favs) ✗ git commit -m "K: Added favs.md"
[favs f91a762] K: Added favs.md
 1 file changed, 2 insertions(+)
 create mode 100644 customers/favs.md
➜  megacorp git:(favs) git switch -c favs2
Switched to a new branch 'favs2'
➜  megacorp git:(favs2) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 8 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) nvim .
➜  megacorp git:(main) ✗ git add customers/favs.md
➜  megacorp git:(main) ✗ git commit -m "L: Added favs.md on main"
[main ac17792] L: Added favs.md on main
 1 file changed, 2 insertions(+)
 create mode 100644 customers/favs.md

# The changes from L and K naturally conflict.
```
## RERERE

```bash
➜  megacorp git:(main) git config set --local rerere.enabled true
➜  megacorp git:(main) git switch favs
Switched to branch 'favs'
➜  megacorp git:(favs) git rebase main
Auto-merging customers/favs.md
CONFLICT (add/add): Merge conflict in customers/favs.md
error: could not apply f91a762... K: Added favs.md
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config set advice.mergeConflict false"
Recorded preimage for 'customers/favs.md' # <--- This Line
Could not apply f91a762... # K: Added favs.md
```

- Notice how it's recording what we're doing as we resolve the conflict? This is the rerere feature in action!
```bash
# Accepted both changes
➜  megacorp git:(ac17792) ✗ git add .
# git may automatically open a file on your editor in which you can edit the commit message, simply save and close to continue
# ![[Git-rerere.png]]
➜  megacorp git:(ac17792) ✗ git rebase --continue
Recorded resolution for 'customers/favs.md'. # <--- This Line
[detached HEAD 07621ae] K: Added favs.md
 1 file changed, 1 insertion(+)
Successfully rebased and updated refs/heads/favs.

➜  megacorp git:(favs) git switch favs2
Switched to branch 'favs2'
➜  megacorp git:(favs2) git rebase main
Auto-merging customers/favs.md
CONFLICT (add/add): Merge conflict in customers/favs.md
error: could not apply f91a762... K: Added favs.md
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config set advice.mergeConflict false"
Staged 'customers/favs.md' using previous resolution.
Could not apply f91a762... # K: Added favs.md
➜  megacorp git:(ac17792) ✗ git rebase --continue
[detached HEAD f579877] K: Added favs.md
 1 file changed, 1 insertion(+)
Successfully rebased and updated refs/heads/favs2
# ![[Git-rerere-2.png]]
```
- If you mess up and need to repeat parts of this step, you can clear the rerere cache with this command from the root of your repo:
	- `rm -rf .git/rr-cache`
## Merge back in
```bash
➜  megacorp git:(favs2) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 9 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) git branch -d favs2
error: the branch 'favs2' is not fully merged
hint: If you are sure you want to delete it, run 'git branch -D favs2'
hint: Disable this message with "git config set advice.forceDeleteBranch false"
➜  megacorp git:(main) git branch -D favs2
Deleted branch favs2 (was f579877).
➜  megacorp git:(main) git merge favs
Updating ac17792..07621ae
Fast-forward
 customers/favs.md | 1 +
 1 file changed, 1 insertion(+)
➜  megacorp git:(main) git branch -d favs
Deleted branch favs (was 07621ae).
```

## Accidental Commit
- You know how with merge conflicts you commit the resolution, 
- but with rebase conflicts you `--continue` the resolution?
- If you accidentally commit the resolution of a rebase conflict, just:
	- `git reset --soft HEAD~1`
- The `--soft` flag will keep your changes, and just undo the commit.
- Then you can simply `--continue` the rebase as normal.

# Squash
- If you join a team that prefers a single commit, you will need to know how to "squash" your commits together. 
- Squashing is useful to keep your commit history clean.
- We take all the changes from a series of commits and squash them into a single commit.

![[Git-sqush.png|500]]

## How to squash
- Squashing is done with the git rebase command
- Here are the steps to squash the last n commits:
	- Start an interactive rebase with the command `git rebase -i HEAD~n`, where n is the number of commits you want to squash.
	- Git will open your default editor with a list of commits.
	- Change the word pick to squash for all but the first commit.
	- Save and close the editor.
- The `-i` flag stands for "interactive," and it allows us to edit the commit history before Git applies the changes. 
- `HEAD~n` is how we reference the last n commits.
	- HEAD` points to the current commit (as long as we're in a clean state) 
	- `~n` means "n commits before HEAD."
- Why Rebase does Squash?
	- Rebase is all about replaying changes.
	- When we rebase onto a specific commit (`HEAD~n`), we're telling Git to replay all the changes from the current branch on top of that commit. 
	- Then we use the interactive flag to "squash" those changes so that Rebase will apply them all in a single commit.
- Squashing is a destructive operation

```bash
➜  megacorp git:(temp_main) git rebase -i HEAD~3
[detached HEAD 0999d72] J: redacted
 Date: Sat Jul 4 23:28:00 2026 +0530
 2 files changed, 6 insertions(+)
 create mode 100644 customers/banned.csv
 create mode 100644 customers/favs.md
Successfully rebased and updated refs/heads/temp_main.
```
- Change pick to squash except `J`.
```bash
pick 39b08d7 # J: Banned.csv from main
squash ac17792 # L: Added favs.md on main
squash 07621ae # K: Added favs.md

# Rebase f760d04..07621ae onto f760d04 (3 commands)

```
- Use only one commit message
```bash
# This is a combination of 3 commits.
# This is the 1st commit message:
J: redacted

# This is the commit message #2:

# This is the commit message #3:


# Date:      Sat Jul 4 23:28:00 2026 +0530

# interactive rebase in progress; onto f760d04
# Last commands done (3 commands done):
#    squash ac17792 # L: Added favs.md on main
#    squash 07621ae # K: Added favs.md
# No commands remaining.
# You are currently rebasing branch 'temp_main' on 'f760d04'.

# Changes to be committed:
#       new file:   customers/banned.csv
#       new file:   customers/favs.md

------------------------ >8 ------------------------
# Do not modify or remove the line above.
# Everything below it will be ignored.
diff --git c/customers/banned.csv i/customers/banned.csv
new file mode 100644
index 0000000..5b13c1a
--- /dev/null
+++ i/customers/banned.csv
@@ -0,0 +1,3 @@
+first_name,last_name,company,title
+Ballan,Agrandian,Boots.lore,Protagonist
+sam,ctrlman,closedai,ceo

diff --git c/customers/favs.md i/customers/favs.md
new file mode 100644
index 0000000..bfe19c6
--- /dev/null
+++ i/customers/favs.md
@@ -0,0 +1,3 @@
+# Favorite Customers
+* Jesse Pinkman, Heisenberg's Assistant
+* Walter White, Heisenberg, Chemist
```

```bash
➜  megacorp git:(temp_main) git log --oneline

0999d72 (HEAD -> temp_main) J: redacted
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 A: The Founding of MegaCorp and the End of Art
```
## Overwrite
- Now that `temp_main` is in the state you want it to be.
- To rename a branch:
	- `git branch -m new-branch-name`

```bash
➜  megacorp git:(temp_main) git branch -d main
warning: deleting branch 'main' that has been merged to
         'refs/remotes/origin/main', but not yet merged to HEAD
Deleted branch main (was 07621ae).
➜  megacorp git:(temp_main) git branch -m main
➜  megacorp git:(main) git branch
```

## Force Push

- We squashed `main` which means that because our remote `main` branch on GitHub has commits that we removed, `git` won't allow us to push our changes because they're totally out of sync.
- The git push command has a `--force` flag that allows us to overwrite the remote branch with our local branch. 
- It's a very dangerous (but useful) flag that overrides the safeguards and just says "make that branch the same as this branch.
- `git push origin main --force`

```bash
➜  megacorp git:(main) git push origin main
Username for 'https://AbhinavGtm':
Password for 'https://AbhinavGt@github.com':
To https://github.com/AbhinavGt/megacorp
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/AbhinavGt/megacorp'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

➜  megacorp git:(main) git push origin main --force
Username for 'https://github.com': AbhinavGt
Password for 'https://AbhinavGt@github.com':
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 582 bytes | 582.00 KiB/s, done.
Total 5 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/AbhinavGt/megacorp
 + 07621ae...0999d72 main -> main (forced update)

```
## Squash is Dangerous
- When you squash commits, into a single commit:

```bash
A - B - C - D
ABCD
```

- you're removing history from the project. 
- Sure, all the changes are still there, but the individual markers of each change are gone.
- Meaning we've erased all the commits we've made in the last few lessons, we can't go back to individual checkpoints anymore.

## Squashing PRs
- If your team prefers single-commit-pull-requests, this will likely be your workflow:
	- Create a new branch off of main.
	- Go about your work on the feature branch making commits as you go.
	- When you're ready to get your code into main, squash all your commits into a single commit.
	- Push your branch to the remote repository.
	- Open a pull request from the feature branch into main.
	- Merge the pull request once it's approved.

```bash
➜  megacorp git:(main) git switch -c add_scanner
Switched to a new branch 'add_scanner'
➜  megacorp git:(add_scanner) ✗ ./scripts/scan.sh
➜  megacorp git:(add_scanner) ✗ git add .
➜  megacorp git:(add_scanner) ✗ git commit -m "K: Credit Card Script"
[add_scanner e6a62c9] K: Credit Card Script
 1 file changed, 3 insertions(+), 1 deletion(-)

➜  megacorp git:(add_scanner) ✗ ./scripts/scan.sh
➜  megacorp git:(add_scanner) ✗ git add .
➜  megacorp git:(add_scanner) ✗ git commit -m "L: Social Security Script"
[add_scanner cbe8cb3] L: Social Security Script
 1 file changed, 3 insertions(+)

➜  megacorp git:(add_scanner) ✗ ./scripts/scan.sh
➜  megacorp git:(add_scanner) ✗ git add .
➜  megacorp git:(add_scanner) ✗ git commit --amend -m "M: Phone Number Script"
[add_scanner 288c89a] M: Phone Number Script
 Date: Sun Jul 5 01:51:30 2026 +0530
 1 file changed, 7 insertions(+)
```
## Squash Series

```bash
➜  megacorp git:(add_scanner) git rebase -i HEAD~3
[detached HEAD 2ec8850] K: Squashed K,L,M commits
 Date: Sun Jul 5 02:13:20 2026 +0530
 1 file changed, 10 insertions(+), 1 deletion(-)
Successfully rebased and updated refs/heads/add_scanner.
```

## Pull Request
- To pull changes from a remote branch to the current local branch:
- `git pull origin BRANCHNAME`


```bash
➜  megacorp git:(add_scanner) git push origin add_scanner
Username for 'https://github.com': AbhinavGt
Password for 'https://AbhinavGt@github.com':
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 602 bytes | 602.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote:
remote: Create a pull request for 'add_scanner' on GitHub by visiting:
remote:      https://github.com/AbhinavGt/megacorp/pull/new/add_scanner
remote:
To https://github.com/AbhinavGt/megacorp
 * [new branch]      add_scanner -> add_scanner
```

![[Git-sq-merge.png]]

```bash
➜  megacorp git:(add_scanner) git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
➜  megacorp git:(main) git branch -D add_scanner
Deleted branch add_scanner (was 2ec8850).
➜  megacorp git:(main) git pull origin main
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 904 bytes | 904.00 KiB/s, done.
From https://github.com/AbhinavGt/megacorp
 * branch            main       -> FETCH_HEAD
   0999d72..f77c7bd  main       -> origin/main
Updating 0999d72..f77c7bd
Fast-forward
 scripts/scan.sh | 11 ++++++++++-
 1 file changed, 10 insertions(+), 1 deletion(-)

➜  megacorp git:(main) git log --oneline

f77c7bd (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from AbhinavGt/add_scanner
2ec8850 (origin/add_scanner) K: Squashed K,L,M commits
0999d72 J: redacted
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 A: The Founding of MegaCorp and the End of Art
```
# Stash
- Here's the workflow you've been using as a junior developer:
	- Commit some changes on your feature branch
	- Open a pull request from your feature branch to main
	- Checkout a new feature branch and start on the next task
- Today however the customers are complaining about a critical bug and the CEO saw you first. She's asked you to drop everything and fix it immediately... but you've got a bunch of unstaged changes that you're not ready to commit yet, and that you don't want to lose. What do?
- The `git stash` command records the current state of your working directory and the index (staging area). 
- It's kinda like your computer's copy/paste clipboard. 
- It records those changes in a safe place and reverts the working directory to match the HEAD commit (the last commit on your current branch).
- To stash your current changes and revert the working directory to match HEAD:
	- `git stash`
- To list your stashes:
	- `git stash list`
```bash
➜  megacorp git:(main) ✗ git stash
Saved working directory and index state WIP on main: f77c7bd Merge pull request #1 from AbhinavGt/add_scanner
➜  megacorp git:(main) git stash list
stash@{0}: WIP on main: f77c7bd Merge pull request #1 from AbhinavGt/add_scanner
```
## Pop
- Stash has a few options, but the ones that you will use most are:
	- `git stash`
	- `git stash pop`
	- `git stash list`
- The `pop` command will (by default) apply your most recent stash entry to your working directory and remove it from the stash list.
- It effectively undoes the `git stash` command. It gets you back to where you were.

```
➜  megacorp git:(main) git stash pop
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (868b8ffe52e584217c0cc15ef7503586b753aeee)
```

## What Is the Stash?
- The git stash command stores your changes in a stack (LIFO) data structure. 
- That means that when you retrieve your changes from the stash, you'll always get the most recent changes first.
-  "stash" is a collection of changes that you've not yet committed. 
- They might just be "raw" working directory changes, or they might be staged changes. 
- Both can be stashed. 
- So, for example, you can:
	- Make some changes to your working directory
	- Stage those changes
	- Make some more changes without staging them
	- Stash all of that
- When you do, the "stash entry" will contain both the staged and unstaged changes and both your working directory and index will be reverted to the state of the last commit. 
- It's a very convenient way to "pause" your work and come back to it later.

![[Git-stash.png]]

## Multiple Stashes
- You can stash changes with a message. 
- If you keep a crazy deep stash then messages are useful, but people usually stash, and then just a few minutes or hours later,  pop it back out. 
- Syntax to provide a message:
	- `git stash -m "jdsl v0 almost working"`
	
```bash
➜  megacorp git:(main) ✗ git stash -m "good marketing"
Saved working directory and index state On main: good marketing
➜  megacorp git:(main) ✗ git stash -m "bad marketing"
Saved working directory and index state On main: bad marketing

➜  megacorp git:(main) git stash list

stash@{0}: On main: bad marketing
stash@{1}: On main: good marketing
```
## Unrelated Commit

```bash
➜  megacorp git:(main) ./scripts/apux.sh ./scripts/scan.sh | grep ' sh'
./customers/unstructured.txt:82:Mia Gray, mia.gray@example.com, Phone: (555) 567-8901, favorite language: sh (bourne shell)
./customers/unstructured.txt:99:Daniel Carter, daniel.carter@example.com, Phone: (555) 456-7890, sh user
➜  megacorp git:(main) nvim .
➜  megacorp git:(main) ✗ ./scripts/apux.sh ./scripts/scan.sh | grep 'apux'
./customers/unstructured.txt:53:Ella Thompson, SSN: 123-45-6789 ella.thompson@example.com, Memberapuxip Status: Inactive
./customers/unstructured.txt:82:Mia Gray, mia.gray@example.com, Phone: (555) 567-8901, favorite language: apux (bourne apuxell)
./customers/unstructured.txt:99:Daniel Carter, daniel.carter@example.com, Phone: (555) 456-7890, apux user
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "L: Updated apux.sh"
[main e351395] L: Updated apux.sh
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## Pop Again
### Apply Without Removing from Stash
- This will apply the most recent stash changes, just like pop, but it will keep the stash in the stash list.
	- `git stash apply`
### Remove a Stash Without Applying
- This will remove the most recent stash from the stash list without applying it to your working directory.
	- `git stash drop`
### Reference a Specific Stash
- Most stash commands allow you to reference a specific stash by its index.

```
# Apply the third (0, 1, 2) most recent stash
git stash apply stash@{2}

# Remove the third most recent stash
git stash drop stash@{2}
```

```bash

➜  megacorp git:(main) git stash list

stash@{0}: On main: bad marketing
stash@{1}: On main: good marketing

➜  megacorp git:(main) git stash apply stash@{1}
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git stash pop
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Recorded preimage for 'README.md'
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
The stash entry is kept in case you need it again.

➜  megacorp git:(main) ✗ nvim .
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "M: Stashed Changes"
Recorded resolution for 'README.md'.
[main 9fa17df] M: Stashed Changes
 1 file changed, 2 insertions(+), 2 deletions(-)
```
# Revert
- A revert is effectively an anti commit. 
- It does not remove the commit (like reset), but instead creates a new commit that does the exact opposite of the commit being reverted. 
- It undoes the change but keeps a full history of the change and its undoing.
### Using Revert
- To revert a commit, you need to know the commit hash of the commit you want to revert. You can find this hash using git log.
	- `git log`
- Once you have the hash, you can revert the commit using git revert.
	- `git revert <commit-hash>`

```bash
➜  megacorp git:(main) git revert 9fa17df96d56c3b0d5a7f6d5925201b2919578ae
[main 68dce1e] N : Revert M
 1 file changed, 2 insertions(+), 2 deletions(-)

➜  megacorp git:(main) git log --oneline

68dce1e (HEAD -> main) N : Revert M
9fa17df M: Stashed Changes
e351395 L: Updated apux.sh
f77c7bd (origin/main, origin/HEAD) Merge pull request #1 from AbhinavGt/add_scanner
2ec8850 (origin/add_scanner) K: Squashed K,L,M commits
0999d72 J: redacted
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 A: The Founding of MegaCorp and the End of Art
```
## Diff
- The git diff command shows you the differences between commits, the working tree, etc.
- It is use to look at the changes between the current state of my code and the last commit.
- For example:

```bash
# show the changes between the working tree and the last commit
git diff

# show the differences between the previous commit and the current state, including the last commit and uncommitted changes

git diff HEAD~1

# show the change between two commits

git diff COMMIT_HASH_1 COMMIT_HASH_2
```

```bash
➜  megacorp git:(main) git diff 68dce1e 9fa17df

diff --git a/README.md b/README.md
index 13b4fae..f0c36b8 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,3 @@
-# megacorp
+# megacorp | good marketing example

-The starter repo for the [Git 2 course](https://www.boot.dev/learn/learn-git-2) on Boot.dev.
+MegaCorp™ is _the_ enterprise Customer Relationship Management (CRM) software. Not only is it an incredible product, but it also ships a programming language for "MegaCorp developers" that want to build custom features within the MegaCorp™ ecosystem. That language is called "Apux".
```

## Revert vs Reset
- `git reset --soft`: Undo commits but keep changes staged
- `git reset --hard`: Undo commits and discard changes
- `git revert`: Create a new commit that undoes a previous commit
### When to Reset
- If you're working on your own branch, and you're just undoing something you've already committed, say you're cleaning everything up so you can open a pull request, then git reset is probably what you want.

### When to Revert
- However, if you want to undo a change that's already on a shared branch (especially if it's an older change), then git revert is the safer option. It won't rewrite any history, and therefore won't step on your coworkers' toes.
# Cherry Pick
- When you want to yoink a commit from a branch, but you don't want to merge or rebase because you don't want all the commits.
- `git cherry-pick <commit-hash>`

```bash
➜  megacorp git:(main) git switch -c add_partners
Switched to a new branch 'add_partners'
➜  megacorp git:(add_partners) nvim .
➜  megacorp git:(add_partners) ✗ git add .
➜  megacorp git:(add_partners) ✗ git commit -m "O: Partners.txt Change1"
[add_partners af47c5a] O: Partners.txt Change1
 1 file changed, 1 insertion(+)
➜  megacorp git:(add_partners) nvim .
➜  megacorp git:(add_partners) ✗ git add .
➜  megacorp git:(add_partners) ✗ git commit -m "P: Partners.txt Change2"
[add_partners d4247b4] P: Partners.txt Change2
 1 file changed, 1 insertion(+)
```

## How to Cherry Pick
- First, you need a clean working tree (no uncommitted changes).
- Identify the commit you want to cherry-pick, typically by git loging the branch it's on.
- Run:
	- `git cherry-pick <commit-hash>`

```bash
➜  megacorp git:(add_partners) git log --oneline

d4247b4 (HEAD -> add_partners) P: Partners.txt Change2
af47c5a O: Partners.txt Change1
68dce1e N : Revert M
9fa17df M: Stashed Changes
e351395 L: Updated apux.sh
f77c7bd (origin/main, origin/HEAD) Merge pull request #1 from AbhinavGt/add_scanner
2ec8850 (origin/add_scanner) K: Squashed K,L,M commits
0999d72 J: redacted
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 A: The Founding of MegaCorp and the End of Art

➜  megacorp git:(add_partners) af47c5a
➜  megacorp git:(add_partners) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) git cherry-pick af47c5a
[main 751ae77] O: Partners.txt Change1
 Date: Sun Jul 5 18:35:39 2026 +0530
 1 file changed, 1 insertion(+)
➜  megacorp git:(main) git log --oneline

751ae77 (HEAD -> main) O: Partners.txt Change1
68dce1e N : Revert M
9fa17df M: Stashed Changes
e351395 L: Updated apux.sh
f77c7bd (origin/main, origin/HEAD) Merge pull request #1 from AbhinavGt/add_scanner
2ec8850 (origin/add_scanner) K: Squashed K,L,M commits
0999d72 J: redacted
f760d04 H: Resolved multi-conflict
58cef71 F: Multi-conflict del_rec
a63ba0e G: Multi-conflict main
0ef8127 E: fine, Greg can have this one
119e1d9 D: add jayson to customers
9fcf449 C: update customers and partners
20b7194 B: recovery
0d16f95 A: The Founding of MegaCorp and the End of Art
➜  megacorp git:(main) git branch -D add_partners
Deleted branch add_partners (was d4247b4).
```

# Bisect
- To fix problems in our code. We can either:
	1. Revert the commit with the bug (this is more common on large teams)
	2. "Fail forward" by just writing a new commit that fixes the bug (this is more common on small teams)
- How do we find out when a bug was introduced?
- Instead of manually checking all the commits, git bisect allows us to do a binary search to find the commit that introduced the bug.
- For example, if you have 100 commits that might contain the bug, with git bisect you only need to check 7 commits to find the one that introduced the bug.

> [!note] Note 
> `git bisect` isn't just for bugs, it can be used to find the commit that introduced any change, but issues like bugs and performance regressions are a common use case.

## How to bisect

- There are effectively 7 steps to bisecting:
    - Start the bisect with `git bisect start`
    - Select a "good" commit with `git bisect good <commitish>` (a commit where you're sure the bug wasn't present)
	- Select a bad commit via `git bisect bad <commitish>` (a commit where you're sure the bug was present)
	- Git will checkout a commit between the good and bad commits for you to test to see if the bug is present
	- Execute `git bisect good` or `git bisect bad` to say the current commit is good or bad
	- Loop back to step 4 (until `git bisect` completes)
	- Exit the bisect mode with `git bisect reset`

### Bisecting
```bash
➜  megacorp git:(main) git bisect start
status: waiting for both good and bad commits
➜  megacorp git:(main) git bisect bad 751ae77
status: waiting for good commit(s), bad commit known
➜  megacorp git:(main) git bisect good 0d16f95
Bisecting: 6 revisions left to test after this (roughly 3 steps)
[f760d04123cad2c5fb51abc813dada92f8ba2419] H: Resolved multi-conflict
➜  megacorp git:(f760d04) git bisect good f760d04
Bisecting: 3 revisions left to test after this (roughly 2 steps)
[f77c7bde2d2310478aa4c2b531338b859652845e] Merge pull request #1 from AbhinavGt/add_scanner
➜  megacorp git:(f77c7bd) git bisect bad f77c7bd
Bisecting: 0 revisions left to test after this (roughly 1 step)
[2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba] K: Squashed K,L,M commits
➜  megacorp git:(2ec8850) git bisect bad 2ec8850
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[0999d728ace1b9fbc4a2d691283dff04b58884ea] J: redacted
➜  megacorp git:(0999d72) git bisect good 0999d72
2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba is the first bad commit
commit 2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba (origin/add_scanner)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Sun Jul 5 02:13:20 2026 +0530

    K: Squashed K,L,M commits

 scripts/scan.sh | 11 ++++++++++-
 1 file changed, 10 insertions(+), 1 deletion(-)
```
### Show commit
```bash
➜  megacorp git:(0999d72) git show  2ec8850

commit 2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba (origin/add_scanner)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Sun Jul 5 02:13:20 2026 +0530

    K: Squashed K,L,M commits

diff --git a/scripts/scan.sh b/scripts/scan.sh
index f4d9eb2..be676a7 100755
--- a/scripts/scan.sh
+++ b/scripts/scan.sh
@@ -1 +1,10 @@
-# TODO: write the script
+printf "\n====== SCANNING FOR CREDIT CARD NUMBERS ======\n"
+grep -rE --color=always '(\b[0-9]{4}[- ]?){3}[0-9]{4}\b' . --exclude-dir={.git} --line-number
+echo "========= CREDIT CARD SCAN COMPLETE =========="
+printf "\n==== SCANNING FOR SOCIAL SECURITY NUMBERS ====\n"
+grep -rE --color=always '\b[0-9]{3}-[0-9]{2}-[0-9]{4}\b' . --exclude-dir={.git} --line-number
+echo "======= SOCIAL SECURITY SCAN COMPLETE ========"
+printf "\n========= SCANNING FOR PHONE NUMBERS =========\n"
+grep -rE --color=always '\b[0-9]{3}-[0-9]{3}-[0-9]{4}\b' . --exclude-dir={.git} --line-number
+grep -rE --color=always '\([0-9]{3}\) [0-9]{3}-[0-9]{4}' . --exclude-dir={.git} --line-number
+echo "========= PHONE NUMBER SCAN COMPLETE ========="
```
### Reset and Revert
```bash
➜  megacorp git:(0999d72) git bisect reset
Previous HEAD position was 0999d72 J: redacted
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 4 commits.
  (use "git push" to publish your local commits)
  
➜  megacorp git:(main) git revert 0999d72
[main 28afcf9] P: Reverting to J
 2 files changed, 6 deletions(-)
 delete mode 100644 customers/banned.csv
 delete mode 100644 customers/favs.md
```

> [!note] Command
>  the `git blame` command can be used to see who made the change, not just when it was made.

## Automation
- Manually running automated tests can take long thus we create a automation script
- From man git-bisect:
```
Bisect run
If you have a script that can tell if the current source code is good or bad, you can bisect by issuing the command:
	$ git bisect run my_script arguments
Note that the script (my_script in the above example) should exit with code 0 if the current source code is good/old, and exit with a code between 1 and 127 (inclusive), except 125, if the current source code is bad/new.
```

```bash
➜  megacorp git:(main) ✗ cat scripts/bisect.sh
if grep -q "SCANNING" "scripts/scan.sh"; then
    exit 1
else
    exit 0
fi

➜  megacorp git:(main) ✗ chmod +x scripts/bisect.sh
➜  megacorp git:(main) ✗ git bisect start
status: waiting for both good and bad commits
➜  megacorp git:(main) ✗ git bisect bad HEAD
status: waiting for good commit(s), bad commit known
➜  megacorp git:(main) ✗ git bisect good 0d16f95
Bisecting: 7 revisions left to test after this (roughly 3 steps)
[f760d04123cad2c5fb51abc813dada92f8ba2419] H: Resolved multi-conflict
➜  megacorp git:(f760d04) ✗ git bisect run ./scripts/bisect.sh
running './scripts/bisect.sh'
Bisecting: 3 revisions left to test after this (roughly 2 steps)
[e351395174b44223cf2ed5ad5afa649e01daf8fe] L: Updated apux.sh
running './scripts/bisect.sh'
Bisecting: 1 revision left to test after this (roughly 1 step)
[2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba] K: Squashed K,L,M commits
running './scripts/bisect.sh'
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[0999d728ace1b9fbc4a2d691283dff04b58884ea] J: redacted
running './scripts/bisect.sh'
2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba is the first bad commit
commit 2ec8850101e2e7b2c40b2c7f5629ee90414eb8ba
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Sun Jul 5 02:13:20 2026 +0530

    K: Squashed K,L,M commits

 scripts/scan.sh | 11 ++++++++++-
 1 file changed, 10 insertions(+), 1 deletion(-)
bisect found first bad commit
➜  megacorp git:(0999d72) ✗ git bisect reset
Previous HEAD position was 0999d72 J: redacted
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 5 commits.
  (use "git push" to publish your local commits)
```

# Worktrees

- You can have more than one working tree.
- A worktree (or "working tree" or "working directory") is just the directory on your filesystem where the code you're tracking with Git lives. 
- Usually, it's just the root of your Git repo (where the .git directory is). 
- It contains:
    - Tracked files (files that Git knows about)
    - Untracked files (files that Git doesn't know about)
    - Modified files (files that Git knows about that have been changed since the last commit)
## The Worktree Command
- Git has the git worktree command that allows us to work with worktrees.
- `git worktree list`
- It lists all the worktrees you created.

```bash
➜  megacorp git:(main) ✗ git worktree list
/home/a8hi9t/git/megacorp 28afcf9 [main]
```

## Linked Worktrees
- The goal is allow you to work on different changes without losing work, but are particularly useful when:
	- You want to switch back and forth between the two change sets without having to run a bunch of git commands (not branches or stash)
	- You want to keep a light footprint on your machine that's still connected to the main repo (not clone)
### The Main Worktree
- Contains the `.git` directory with the entire state of the repo 
- Heavy (lots of data in there!). To get a new main working tree requires a git clone or git init
### A Linked Worktree
- Contains a `.git` file with a path to the main working tree
- Light (essentially no data in there!), about as light as a branch
- Can be complicated to work with when it comes to `env` files and `secrets`
### Create a Linked Worktree
- To create a new worktree at a given path:
	- `git worktree add <path> [<branch>]`
	- Adding `<branch>` is optional. 
	- It will use the last part of the path as the branch name.

```bash
➜  megacorp git:(main) ✗ git switch -c ultracorp
Switched to a new branch 'ultracorp'
➜  megacorp git:(main) ✗ git worktree add ../ultracorp ultracorp
Preparing worktree (checking out 'ultracorp')
➜  megacorp git:(main) ✗ cat ../ultracorp/.git
gitdir: /home/a8hi9t/git/megacorp/.git/worktrees/ultracorp
➜  megacorp git:(main) ✗ git worktree list
/home/a8hi9t/git/megacorp  28afcf9 [main]
/home/a8hi9t/git/ultracorp 28afcf9 [ultracorp]
```

## No duplicate branches
- Linked worktrees behave just like a "normal" git repo. 
- You can create new branches, switch branches, delete branches, create tags, etc etc.
- BUT there is one thing you cannot do... you cannot work on a branch that is currently checked out by any other working tree (main or linked).

```bash
➜  megacorp git:(main) ✗ cd ../ultracorp
➜  ultracorp git:(ultracorp) git switch main
fatal: 'main' is already used by worktree at '/home/a8hi9t/git/megacorp'
```
## Tracking
- So how does your main worktree know about your linked worktree?
	- Well, the references are stored in the `.git/worktrees` directory!
```bash
➜  ultracorp git:(ultracorp) cd ../megacorp
➜  megacorp git:(main) ✗ ls .git/worktrees
ultracorp
```

## Upstream
- When you make a change in a `linked` worktree, that change is automatically reflected in the `main` worktree!
- It makes sense: the linked worktree doesn't have a `.git` directory, so it's not a separate repository. It's just a different view of the same repository.
- You can almost think of a linked worktree as just another branch in the same repo, but with its own space on the filesystem.
## Delete Worktrees
- Stash is still useful for tiny stuff, but worktrees are so much better for long-lived changes.
- However, at some point you will need to clean up your worktrees. 
- The simplest way is the remove subcommand:
	- `git worktree remove WORKTREE_NAME`
- An alternative is to delete the directory manually, then prune all the worktrees (removing the references to deleted directories):
	- `git worktree prune`
```bash
➜  megacorp git:(main) ✗ git worktree remove ultracorp
➜  megacorp git:(main) ✗ git branch
* main        ultracorp
  
➜  megacorp git:(main) ✗ ls ..
megacorp  webflyx  webflyx-local
```

# Tags
- A tag is a name linked to a commit that doesn't move between commits, unlike a branch. 
- Tags can be created and deleted, but not modified.
## How to Tag
- To list all current tags:
	- `git tag`
- To create a tag on the current commit:
	- `git tag -a "tag name" -m "tag message"`

```bash
➜  megacorp git:(main) ✗ git tag -a "candidate" -m "This is a tag"
➜  megacorp git:(main) ✗ git tag
candidate
```

## Semver
- "Semver", or "Semantic Versioning", is a naming convention for versioning software. 
- You've probably seen it around, it looks like this:
	- `v3.12.5`
	- `v "major breaking changes"."minor safe features."patch safe bug fixes"`

>[!note] Fact
> The "v" isn't technically part of "semver", but it's often there to say "this is a version

- It has two primary purposes:
    - To give us a standard convention for versioning software
    - To help us understand the impact of a version change and if it's safe (how hard it will be) to upgrade to
- Each part is a number that starts at 0 and increments upward forever. 
- The rules are simple:
	- MAJOR increments when we make "breaking" changes (this is typically a big release, for example, Python 2 -> Python 3)
	- MINOR increments when we add new features in a backward-compatible manner
	- PATCH increments when we make backward-compatible bug fixes
- To sort them from highest to lowest, you first compare the major versions, then the minor versions, and finally the patch versions. 
- For example, a major version of 2 is always greater than a major version of 1, regardless of the minor and patch versions.
- As a special case, major version 0 is typically considered to be pre-release software and thus the rules are more relaxed.
## Conventional Tags
- Tags are used for all sorts of reasons, but sometimes they're used to denote releases. 
- In that case, tags that follow semver are common.
- To tag:
	- `git tag -a v3.10.2 -m "Fixed a lil bug"`
- Pretty much anywhere you can use a commit hash, you can use a tag name when working with the git CLI.
-  Commits can have multiple tags.
- you can push your tags up to your remote GitHub repo
	- `git push origin --tags`
```bash
➜  megacorp git:(main) ✗ git push origin --tags
Username for 'https://github.com': AbhinavGt
Password for 'https://AbhinavGt@github.com':
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 12 threads
Compressing objects: 100% (15/15), done.
Writing objects: 100% (17/17), 1.90 KiB | 1.90 MiB/s, done.
Total 17 (delta 6), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (6/6), completed with 1 local object.
To https://github.com/AbhinavGt/megacorp
 * [new tag]         candidate -> candidate
 * [new tag]         v1.0.0 -> v1.0.0
```

