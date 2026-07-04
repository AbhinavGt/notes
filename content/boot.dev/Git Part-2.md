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
- This is the standard way to contribute to someone else's open-source project. The steps are typically:
	1. Fork their repo into your account
	2. Clone your fork to your local machine
	3. Create a new branch (let's call it `your_feature`)
	4. Make changes
	5. Commit and push changes to your fork's remote your_feature branch
	6. Create a pull request to `original_owner/repo` main from `your_username/repo your_feature`
- Then the original owner can review your changes.
- If they like them, they can merge the changes straight from your fork into their repository

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
[delete_records ac96e11] F: Multi-conflict
 2 files changed, 1 insertion(+), 2 deletions(-)
➜  megacorp git:(delete_records) git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 4 commits.
  (use "git push" to publish your local commits)
➜  megacorp git:(main) nvim
➜  megacorp git:(main) ✗ git add .
➜  megacorp git:(main) ✗ git commit -m "G: Multi-conflict main"
[main f9e59a1] G: Multi-conflict main w
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
