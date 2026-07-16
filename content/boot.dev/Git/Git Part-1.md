- Git is the distributed version control system (VCS)
- Git is a command-line tool
- `git help git` for official Git manual
- In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. 
	- The porcelain commands are the ones that you will use most often as a developer to interact with your code.
		- `git status, git add, git commit, git push, git pull, git log`
	- The plumbing commands are low-level commands designed for internal operations and are not typically used directly by end-users.
		- `git apply, git commit-tree, git hash-object`
- Configuration 
	- Whenever code changes, Git tracks who made the change.
	- Git comes with a configuration both at the global and the repo (project) level.
		- `git config get user.name` -> to get the username
		- `git config set --global user.name "github_username_here"` -> to set the username globally
		- `git config get user.email` -> to get the email
		- `git config set --global user.email "email@example.com"` -> to set the email globally
		- `git config set --global init.defaultBranch master` -> to set default branch globally
- `master`  branch is Git's default branch and `main` branch is GitHub's default branch.
- Your `~/.gitconfig` file is the file that stores your global Git configuration
# config
- The very first step of any project is to create a repository.
- A Git "repository" (or "repo") represents a single project.
- A repo is essentially just a directory that contains a project (other directories and files).
- The only difference is that it _also_ contains a hidden `.git` directory. 
- That hidden directory is where Git stores all of its internal tracking and versioning information for the project.

``` bash
➜ ~ mkdir webflyx 
cd webflyx
➜ webflyx git init 
Initialized empty Git repository in /home/a8hi9t/webflyx/.git/
➜ webflyx git:(master) ls -a
. .. .git
➜ webflyx git:(master) ls .git 
config description HEAD hooks info objects refs
```

# Status
- A file can be in one of several states in a Git repository.
	-  `untracked`: Not being tracked by Git
	- `staged`: Marked for inclusion in the next commit
	- `committed`: Saved to the repository's history
- `git status` command shows you the current state of your repo.
- It will tell you which files are untracked, staged, and committed.

```bash
➜ webflyx git:(master)✗ git status 
On branch master 
No commits yet 
Untracked files: 
	(use "git add <file>..." to include in what will be committed)
		contents.md
nothing added to commit but untracked files present (use "git add" to track)
➜ webflyx git:(master) ✗ cat contents.md 
# contents
```

# Staging
- The `contents.md` file has been created, it's `_untracked_`. Thus we stage it.
- `git add <path-to-file OR pattern>` -> to stage files
- Without staging, no files are included in the commit – only the files you explicitly git add will be committed.

```bash
➜ webflyx git:(master) ✗ git add contents.md
➜ webflyx git:(master) ✗ git status 
On branch master 
No commits yet 
Changes to be committed: 
	(use "git rm --cached <file>..." to unstage)
		new file: contents.md
```

# Commit
- After staging a file, we can commit it.
- A commit is a snapshot of the repository at a given point in time.
- It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. 
- A commit comes with a message that describes the changes made in the commit.
	- `git commit -m "your message here"`
- If you screw up a commit message, you can change it with the `--amend` flag.
	- `git commit --amend -m "A: add contents.md"`
	- 

# Git log
- A Git repo is a (potentially very long) list of commits, where each commit represents the _full state of the repository_ at a given point in time.
- The `git log` command shows a history of the commits in a repository. 
- This is what makes Git a version control system. You can see:
	- Who made a commit
	- When the commit was made
	- What was changed
## Commit Hash
- Each commit has a unique identifier called a "commit hash". 
- This is a long string of characters that uniquely identifies the commit
- For convenience, you can refer to any commit or change within Git by using the first `7` characters of its hash. 
- `5ba786fcc93e8092831c01e71444b9baa2228a4f` ->  `5ba786f`
- Notice that `git log` (assuming the log is long enough) starts an interactive pager.
- You can scroll through the log with the arrow keys, and exit by pressing `q`.
- We can use the `-n` and `--no-pager` options to limit the maximum number of commits shown

```bash
➜ webflyx git:(master) git --no-pager log -n 10
commit 0045b58183e450fe8d66e8a14edfaa1f7a81f283 (HEAD -> master)
Author: Victon18 <abhinavbatham333@gmail.com> 
Date: Wed Jun 24 13:53:49 2026 +0530 
	A: add contents.md
```

# Git internals
## Different Hashes
- While commit hashes _are_ derived from their content changes, there's also some other stuff that affects the end hash. For example:
	- The commit message
	- The author's name and email
	- The date and time
	- Parent (previous) commit hashes
- Git uses a cryptographic hash function called SHA-1 to generate commit hashes
- While SHA-1 collisions are possible under contrived conditions, you won't accidentally create two different commits with the same hash, and because they're generated automatically for you.
- Commit hashes are also referred to as "SHAs".

## The plumbing
- All the data in a Git repository is stored directly in the (hidden) `.git` directory. 
- That includes all the commits, branches, tags, and other objects
- Git is made up of objects that are stored in the `.git/objects` directory. A commit is just a type of object.

- Use `ls -al .git/objects/XX/` (replacing `XX` with your two characters of your commit hash) to list the contents of that directory. 
- You should see a file whose name is the remaining characters of the hash

```bash
➜ webflyx git:(master) git --no-pager log -n 10
commit 0045b58183e450fe8d66e8a14edfaa1f7a81f283 (HEAD -> master)
Author: Victon18 <abhinavbatham333@gmail.com> 
Date: Wed Jun 24 13:53:49 2026 +0530 
	A: add contents.md

➜ webflyx git:(master) ls -l .git/objects 
total 24 
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:54 00
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:53 3c
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:53 5b
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:49 ef
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:35 info
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:35 pack

➜ webflyx git:(master) ls -al .git/objects/00/ 
total 12 
drwxr-xr-x 2 a8hi9t a8hi9t 4096 Jun 24 13:54 .
drwxr-xr-x 8 a8hi9t a8hi9t 4096 Jun 24 13:54 ..
-r--r--r- 1 a8hi9t a8hi9t 157 Jun 24 13:54 45b58183e450fe8d66e8a14edfaa1f7a81f283
```

## The Object File
- Try to `cat` the contents of the commit object file. The contents have been compressed to raw bytes!
- The `xxd` command can print the contents of the file in hexadecimal format

```bash
➜  webflyx git:(master) xxd .git/objects/00/45b58183e450fe8d66e8a14edfaa1f7a81f283 > ~/commit_object_hex.txt
➜  ~ cat commit_object_hex.txt
00000000: 7801 4bce cfcd cd2c 5130 b434 6628 294a  x.K....,Q0.4f()J
00000010: 4d55 304d 3232 4c31 4933 344b 3449 3230  MU0M22L1I34K4I20
00000020: 4f34 4b4e 4935 4d34 3632 3132 b430 4f33  O4KNI5M46212.0O3
00000030: 4b34 4d34 b348 3230 3348 e34a 2c2d c9c8  K4M4.H203H.J,-..
00000040: 2f52 08cb 4c2e c9cf 33b4 50b0 494c cac8  /R..L...3.P.IL..
00000050: cc4b 2c4b 4a2c c948 cc35 3636 7648 cf4d  .K,KJ,.H.566vH.M
00000060: cccc d14b cecf b553 3034 b730 32b2 b034  ...K...S04.02..4
00000070: 31b2 54d0 3630 3536 e082 585b 924a aa01  1.T.6056..X[.J..
00000080: a6c6 5003 b81c ad14 1253 5214 92f3 f34a  ..P......SR....J
00000090: 52f3 4a8a f572 53b8 00df 433d c4         R.J..rS...C=.
```

## Cat File
- `cat-file` command allows us to see the contents of a commit without needing to futz around with the object files directly.
- `git cat-file -p <hash>`

```bash
➜ webflyx git:(master) git cat-file -p 0045b58183e450fe8d66e8a14edfaa1f7a81f283 > ~/catfileout.txt
➜  ~ cat catfileout.txt
tree 5b21d4f16a4b07a6cde5a3242187f6a5a68b060f
author Victon18 <abhinavbatham333@gmail.com> 1782289429 +0530
committer Victon18 <abhinavbatham333@gmail.com> 1782289453 +0530
	A: add contents.md
```

## Trees and blobs

- tree: git's way of storing a directory
- blob: git's way of storing a file

```
tree 5b21d4f16a4b07a6cde5a3242187f6a5a68b060f
author Victon18 <abhinavbatham333@gmail.com> 1782289429 +0530
committer Victon18 <abhinavbatham333@gmail.com> 1782289453 +0530
	A: add contents.md
```

- we can see:
	- The `tree` object
	- The `author`
	- The `committer`
	- The commit message
-  We cannot see the contents of the `contents.md` file itself! That's because the blob object stores it.
- Using command with the hash of the `tree` object instead of the commit hash. You should see a `blob` object with its own hash.
- using command with the `blob` object's hash to view its contents.

``` bash
➜ webflyx git:(master) git cat-file -p 5b21d4f16a4b07a6cde5a3242187f6a5a68b060f 
100644 blob ef7e93fc61a91deecaa551c4707e4c3049af42c9 contents.md

➜ webflyx git:(master) git cat-file -p ef7e93fc61a91deecaa551c4707e4c3049af42c9 
# contents
```

## Second Commit
- a new field parent is added to the cat-file output  that signifies the last commit 

``` bash
➜ webflyx git:(master) ✗ git add titles.md
➜ webflyx git:(master) ✗ git commit -m "B: add titles" 
[master 5b8e09f] B: add titles 
1 file changed, 7 insertions(+)
create mode 100644 titles.md
➜ webflyx git:(master) git --no-pager log -n 10
commit 5b8e09f79b410594e49cdc714761eea194ea2c82 (HEAD -> master)
Author: Victon18 <abhinavbatham333@gmail.com> 
Date: Thu Jun 25 11:32:56 2026 +0530 
	B: add titles
commit 0045b58183e450fe8d66e8a14edfaa1f7a81f283
Author: Victon18 <abhinavbatham333@gmail.com> 
Date: Wed Jun 24 13:53:49 2026 +0530 
	A: add contents.md
➜ webflyx git:(master) git cat-file -p 5b8e09f79b410594e49cdc714761eea194ea2c82 
tree 37712fab0aa902b37e2197ebb71e4e1ba4f45f3d
parent 0045b58183e450fe8d66e8a14edfaa1f7a81f283 
author Victon18 <abhinavbatham333@gmail.com> 1782367376 +0530 
committer Victon18 <abhinavbatham333@gmail.com> 1782367376 +0530 

B: add titles
➜  webflyx git:(master) ✗ git cat-file -p 37712fab0aa902b37e2197ebb71e4e1ba4f45f3d
100644 blob ef7e93fc61a91deecaa551c4707e4c3049af42c9    contents.md
100644 blob 66693b8d72daef9170108c6ab4b3abd7f3950dd2    titles.md
```

## Storing Data
- While it's true that Git stores entire snapshots, it does have some performance optimizations so that your `.git` directory doesn't get too unbearably large.
	- Git compresses and packs files to store them more efficiently.
	- Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git will only store it once.
	
```bash
➜  webflyx git:(master) ✗ git add .
➜  webflyx git:(master) ✗ git commit -m "C: add quotes"
[master b4f0c27] C: add quotes
2 files changed, 9 insertions(+)
create mode 100644 quotes/dune.md
create mode 100644 quotes/starwars.md
➜  webflyx git:(master) git cat-file -p b4f0c2722f2b3c4ab995eecf40d49fc9b0ccf8bc
tree 1e5372bb7147aab453239bc790aacd9fd1b9f785
parent 5b8e09f79b410594e49cdc714761eea194ea2c82
author Victon18 <abhinavbatham333@gmail.com> 1782373090 +0530
committer Victon18 <abhinavbatham333@gmail.com> 1782373090 +0530

C: add quotes

➜  webflyx git:(master) git cat-file -p 1e5372bb7147aab453239bc790aacd9fd1b9f785
100644 blob ef7e93fc61a91deecaa551c4707e4c3049af42c9    contents.md
040000 tree 18d53b21a68cf7f8d8b4fb2c293ed409fb967cd3    quotes
100644 blob 66693b8d72daef9170108c6ab4b3abd7f3950dd2    titles.md
```

# Config
- Git stores author information so that when you're making a commit it can track who made the change

```bash
git config set --global user.name "ThePrimeagen"
git config set --global user.email "the.primeagen@aol.com"
```

- Let's take the command apart:
	- `git config`: The command to interact with your Git configuration.
	- `set`: The subcommand to set a value – i.e., to add it if it doesn't already exist, or update it if it does.
	- `--global`: Flag stating you want this configuration to be stored globally in your `~/.gitconfig`. 
	- The. opposite is `--local`, which stores the configuration in the current repository only.
	- `user`: The section.
	- `name`: The key within the section.
	- `"ThePrimeagen"`: The value you want to set for the key.
- `git config list --local` -> to view the contents of your config
- `cat .git/config` -> to just view the contents of your local config file directly

```bash
➜  webflyx git:(master) git config set webflyx.ceo "ThePrimeagen"
➜  webflyx git:(master) git config set webflyx.cto "TheLaneagen"
➜  webflyx git:(master) git config set webflyx.valuation "mid"
➜  webflyx git:(master) git config list --local

core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
webflyx.ceo=ThePrimeagen
webflyx.cto=TheLaneagen
webflyx.valuation=mid

➜  webflyx git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[webflyx]
        ceo = ThePrimeagen
        cto = TheLaneagen
        valuation = mid
```
## Get
- the `get` subcommand is useful for getting a single value.
- `git config get <key>`
- Keys are in the format `<section>.<keyname>`.  For example:
	- `user.name`
	- `webflyx.ceo`
## Unset 
- The `unset` subcommand is used to remove a configuration value.
- `git config unset <key>`

```bash
➜  webflyx git:(master) git config get webflyx.valuation
mid
➜  webflyx git:(master) git config unset webflyx.cto
➜  webflyx git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[webflyx]
        ceo = ThePrimeagen
        valuation = mid
```

##  Duplicates
- Typically, in a key/value store, like a Python dictionary you aren't allowed to have duplicate keys. Strangely enough, Git doesn't care.
### Unset
- The `unset --all` command is useful if you ever _really_ want to purge all instances of a key from your configuration. 
- Conversely, the `unset` subcommand by itself only works with a single instance of a key
- `git config unset --all example.key`
### Append
- Use to add more values to the configuration (note the `--append` flag):

## Removing a section
- The `remove-section` subcommand is used to remove an entire section from your Git configuration.
- `git config remove-section <section>`

 ```bash
 ➜  webflyx git:(master) git config set --append webflyx.ceo "Warren"
git config set --append webflyx.ceo "Carson"
git config set --append webflyx.ceo "Sarah"
➜  webflyx git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[webflyx]
        ceo = ThePrimeagen
        valuation = mid
        ceo = Warren
        ceo = Carson
        ceo = Sarah
➜  webflyx git:(master) git config unset --all webflyx.ceo
➜  webflyx git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[webflyx]
        valuation = mid
➜  webflyx git:(master) git config remove-section webflyx
➜  webflyx git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
 ```

## Locations
- There are several locations where Git can be configured. From more general to more specific, they are:
	- system: `/etc/gitconfig`, a file that configures Git for all users on the system
	- global: `~/.gitconfig`, a file that configures Git for all projects of a user
	- local: `.git/config`, a file that configures Git for a specific project
	- worktree: `.git/config.worktree`, a file that configures Git for part of a project
## Overriding
- If you set a configuration in a more specific location, it will override the same configuration in a more general location
- For example, if you set `user.name` in the local configuration, it will override the `user.name` set in the global configuration.

![[Git-override.png]]

# Branch
- A Git branch allows you to keep track of different changes separately.
- A branch is just a named pointer to a specific commit.
- When you create a branch, you are creating a new pointer to a specific commit.
- The commit that the branch points to is called the tip of the branch.
- Because a branch is just a pointer to a commit, they're lightweight and "cheap" resource-wise to create. 
- When you create 10 branches, you're not creating 10 copies of your project on your hard drive.

![[Git-branch.png | 350]]

```bash
➜  webflyx git:(master) git branch
* master
```
- you should be on `master` because we set `init.defaultBranch` to `master` at the start.
- We've been using Git's default `master` branch. And GitHub's default branch is `main`.
## Renaming Branch
- `git branch -m oldname newname`

```bash
➜  webflyx git:(master) git config set --global init.defaultBranch main
➜  webflyx git:(master) git branch
*master
➜  webflyx git:(master) git branch -m master main
➜  webflyx git:(main) git branch
*main
```
## Visualizing Branches
- `A - B - C  main` means a branch called `main` with 3 commits. `C` is the most recent commit (the tip), `B` is the previous commit, and `A` is the commit before that.

```
    D - E  other_branch
  /
A - B - C  main
```

- `A - D - E other_branch` and `A - B - C main` 

## New Branch
- `git branch my_new_branch`
	- This creates a new branch called `my_new_branch`.
	- This just creates a new branch but not switches to it.
- `git switch -c my_new_branch`
	- The switch command allows you to switch branches. 
	- Including the `-c` flag tells Git to create a new branch and switch to it.
- If you're on your `main` branch with 3 commits, `A`, `B`, and `C`, and then you run `git switch -c my_new_branch`, your new branch will look like this
- Even though we switch the branch commits remain the same.

![[Git-branch-wcommits.png]]

```bash
➜  webflyx git:(main) git switch -c add_classics
Switched to a new branch 'add_classics'
➜  webflyx git:(add_classics) git branch

* add_classics   main

```

## Switching Branches
- `git switch` is a newer command that is meant to be more intuitive and user-friendly. 
- `get checkout` is older way to switch branches
- To switch to a branch called `prime`:

```bash
git switch prime

# or, the old way:
git checkout prime
```

```bash
➜  webflyx git:(main) git switch -c add_classics
Switched to a new branch 'add_classics'
➜  webflyx git:(add_classics) ✗ git add classics.csv
➜  webflyx git:(add_classics) ✗ git commit -m "D: branching to add_classics"
[add_classics b68b934] D: branching to add_classics
 1 file changed, 6 insertions(+)
 create mode 100644 classics.csv
➜  webflyx git:(add_classics) git log

commit b68b934adea52edbc5db6b0df1aa88ca728dbd37 (HEAD -> add_classics)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 15:30:40 2026 +0530

    D: branching to add_classics

commit b4f0c2722f2b3c4ab995eecf40d49fc9b0ccf8bc (main)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 13:08:10 2026 +0530

    C: add quotes

commit 5b8e09f79b410594e49cdc714761eea194ea2c82
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 11:32:56 2026 +0530

    B: add titles

commit 0045b58183e450fe8d66e8a14edfaa1f7a81f283
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Wed Jun 24 13:53:49 2026 +0530

    A: add contents.md

```
## log flags
- There are a few flags that can make the output easier to read.
- Like `--decorate`, etc.
- A ref is just a pointer to a commit. All branches are refs, but not all refs are branches.
```bash
➜  webflyx git:(add_classics) git log --decorate=full

commit b68b934adea52edbc5db6b0df1aa88ca728dbd37 (HEAD -> refs/heads/add_classics)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 15:30:40 2026 +0530

    D: branching to add_classics

commit b4f0c2722f2b3c4ab995eecf40d49fc9b0ccf8bc (refs/heads/main)
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 13:08:10 2026 +0530

    C: add quotes

commit 5b8e09f79b410594e49cdc714761eea194ea2c82
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Thu Jun 25 11:32:56 2026 +0530

    B: add titles

commit 0045b58183e450fe8d66e8a14edfaa1f7a81f283
Author: Victon18 <abhinavbatham333@gmail.com>
Date:   Wed Jun 24 13:53:49 2026 +0530

    A: add contents.md

➜  webflyx git:(add_classics) git log --oneline

b68b934 (HEAD -> add_classics) D: branching to add_classics
b4f0c27 (main) C: add quotes
5b8e09f B: add titles
0045b58 A: add contents.md   
```

## git files
- The "heads" (or "tips") of branches are stored in the `.git/refs/heads` directory.
- If you `cat` one of the files in that directory, you should be able to see the commit hash that the branch points to

```bash
➜  webflyx git:(add_classics) ls .git/refs/heads
add_classics  main
➜  webflyx git:(add_classics) cat .git/refs/heads/add_classics
b68b934adea52edbc5db6b0df1aa88ca728dbd37
```
# Merge

```
Before:
A - B - C    main
   \
    D - E    other_branch
    
After:
A - B - C - F    main
   \     /
    D - E        other_branch
```
- You have two branches, each with their own unique commits:
- If you merge `other_branch` into `main`, Git combines both branches by creating a new commit that has _both_ histories as parents. In the diagram below, `F` is a merge commit that has `C` and `E` as parents. `F` brings all the changes from `D` and `E` back into the `main` branch.

```bash
➜  webflyx git:(add_classics) git switch main
Switched to branch 'main'
➜  webflyx git:(main) ✗ git add .
➜  webflyx git:(main) ✗ git commit -m "E: added contents"
[main 17e04af] E: added contents
 1 file changed, 4 insertions(+)
➜  webflyx git:(main) git log --oneline --graph --all

* 17e04af (HEAD -> main) E: added contents
| * b68b934 (add_classics) D: branching to add_classics
|/
* b4f0c27 C: add quotes
* 5b8e09f B: add titles
* 0045b58 A: add contents.md

```
## Merge commits
- A merge commit is the result of merging two branches together.
```
Before:
A - B - C    main
   \
    D - E    feature_1
    
After:
A - B - C - F    main
   \     /
     D - E        feature_1
```
- If  we merge `feature_1` into main by running this while on main :
	- `git switch main`
	- `git merge feature_1`
- The merge will :
	- Find the "merge base" commit, or "best common ancestor" of the two branches. In this case, A.
    - Replay the changes from main, starting from the best common ancestor, into a new commit.
    - Replay the changes from `feature_1` onto main, starting from the best common ancestor.
    - Records the result as a new commit, in our case, F.
    - F is special because it has two parents, C and E.
- Happy path: keep main checked out. If you want to make the branches diverge more first, add one commit on main and one on feature_1, then use Merge while still on main to create a two-parent merge commit.

```bash
➜  webflyx git:(main) git merge add_classics
Merge made by the 'ort' strategy.
 classics.csv | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 classics.csv
➜  webflyx git:(main) git log --oneline --graph --parents

*   e869db9 17e04af b68b934 (HEAD -> main) F: Merge branch 'add_classics'
|\
| * b68b934 b4f0c27 (add_classics) D: branching to add_classics
* | 17e04af b4f0c27 E: added contents
|/
* b4f0c27 5b8e09f C: add quotes
* 5b8e09f 0045b58 B: add titles
* 0045b58 A: add contents.md
```
## Merge Log

```
*   89629a9 d234104 b8dfd64 (HEAD -> main) F: Merge branch 'add_classics'
|\
| * b8dfd64 fba0999 (tag: 5.8, add_classics) D: add classics
* | d234104 fba0999 (tag: 6.1) E: update contents
|/
* fba0999 1381199 (tag: 3.8, origin/master, origin/main, master) C: add quotes
* 1381199 a21228f (tag: 3.7) B: add titles.md
* a21228f A: add contents.md
```

- Each asterisk `*` represents a commit in the repository. 
- There are multiple commit hashes on each line because the `--parents` flag logs the parent hash(es) as well.
- The first line, with these three hashes: `89629a9 d234104 b8dfd64` is our recent merge commit.
	- The first hash, `89629a9` is the merge commit's hash, and the other two are the parent commits.
- The next section is a visual representation of the branch structure. 
	- It shows the commits on the `add_classics` branch and the `main` branch before the merge. Notice that they both share a common parent.
- The next two lines are just "normal" commits, each pointing to their parent.
- The last line is the initial commit and therefore has no parent.

```bash
➜  megacorp git:(main) ✗ git --no-pager log --oneline --decorate --graph --parents
* 28afcf9 751ae77 (HEAD -> main, tag: v1.0.0, tag: candidate, ultracorp) P: Reverting to J
* 751ae77 68dce1e O: Partners.txt Change1
* 68dce1e 9fa17df N : Revert M
* 9fa17df e351395 M: Stashed Changes
* e351395 f77c7bd L: Updated apux.sh
*   f77c7bd 0999d72 2ec8850 (origin/main, origin/HEAD) Merge pull request #1 from AbhinavGt/add_scanner
|\
| * 2ec8850 0999d72 (origin/add_scanner) K: Squashed K,L,M commits
|/
* 0999d72 f760d04 J: redacted
*   f760d04 a63ba0e 58cef71 H: Resolved multi-conflict
|\
| * 58cef71 0ef8127 F: Multi-conflict del_rec
* | a63ba0e 0ef8127 G: Multi-conflict main
|/
*   0ef8127 9fcf449 119e1d9 E: fine, Greg can have this one
|\
| * 119e1d9 20b7194 D: add jayson to customers
* | 9fcf449 20b7194 C: update customers and partners
|/
* 20b7194 0d16f95 B: recovery
* 0d16f95 A: The Founding of MegaCorp and the End of Art
```
- Git log flag"
	- The first is `--decorate`. It gives the branch and tag information. Lets you see how far you are away from a branch or how far the previous branch might be.
		- It can be one of:
			- `short` (the default)
			- `full` (shows the full ref name)
			- `no` (no decoration)
- `--oneline` flag will show you a more compact view of the log. Hashes are short to 7 characters minimum. Along with commit message
- `--graph` will draw all the lines to see how your commits diverge and merge
- `--parents` will give you parent commit upon every other commit. These are useful for merge commit because you can see where the two parents came from to create the merge commit
##  Fast Forward Merge

- The simplest type of merge is a fast-forward merge.

```
before:
      C     feature_1
     /
A - B       main

after:
            feature_1
A - B - C   main
```

- And we run this while on `main`:
	- `git merge feature_1`
- Because `feature_1` has all the commits that `main` has, Git automatically does a fast-forward merge. 
- It just moves the pointer of the "base" branch to the tip of the "feature" branch
-  With a fast-forward merge, no merge commit is created.
- Happy path: keep `main` behind `feature_1`, then use `Merge` while `main` is checked out. If you add a commit to `main` first, the branches will diverge and it will stop being a fast-forward merge.
- A common workflow when working with Git on a team of developers:
	1. Create a branch for a new change
	2. Make the change
	3. Merge the branch back into the "default" branch
	4. Remove the branch
	5. Repeat

Fast-forward Commit
```bash
➜  webflyx git:(main) git branch -d add_classics
Deleted branch add_classics (was b68b934).
➜  webflyx git:(main) git switch -c update_titles
Switched to a new branch 'update_titles'
➜  webflyx git:(update_titles) ✗ git add titles.md
➜  webflyx git:(update_titles) ✗ git commit -m "G: Updated titles.md"
[update_titles a53d85e] G: Updated titles.md
 1 file changed, 1 insertion(+)
➜  webflyx git:(update_titles) git log --oneline --graph --parents

* a53d85e e869db9 (HEAD -> update_titles) G: Updated titles.md
*   e869db9 17e04af b68b934 (main) F: Merge branch 'add_classics'
|\
| * b68b934 b4f0c27 D: branching to add_classics
* | 17e04af b4f0c27 E: added contents
|/
* b4f0c27 5b8e09f C: add quotes
* 5b8e09f 0045b58 B: add titles
* 0045b58 A: add contents.md
  
➜  webflyx git:(update_titles) git switch main
Switched to branch 'main'
➜  webflyx git:(main) git merge update_titles
Updating 67481ec..51597bf
Fast-forward
 titles.md | 1 +
 1 file changed, 1 insertion(+)
➜  webflyx git:(main) git log --oneline --graph --all

* 51597bf (HEAD -> main, update_titles) G: Updated titles.md
*   67481ec F: Merge branch 'add_classics'
|\
| * 103b051 D: branching to add_classics
* | 0ed790c E: added contents
|/
* 737bcdc C: add quotes
* c8997d4 B: add titles
* 16b6ce3 A: add contents.md
```




# Rebase

- Merge vs Rebase
	- Lets say we have two branch main and feature
	- Commits A, B, C on main branch and D, E on feature branch and branch diverges from commit A.
	- Merge will merge both branches main and feature into main creating a merge commit F
	- Rebase will allow you to move forward the feature branch from diverging at commit A to C.
	- It allows for a fast-forward merge into main. It also allows you to maintain a merge commit free history
- Rebase helps you take diverging commits from one branch and move them to the tip of the base branch that the feature branch is based on. 
- It allows for no merge commit. It simply replays the commits from `feature` on top of `main` for a clean history.
- It is recommend to use rebase  because it allow for a merge commit free history which make some of the advance git features to be more easily useable
## Rebasing a new branch
- We can use the git switch command to create and switch to a new branch called `update_dune` but branch off of the `D` commit.
	- `git switch -c update_dune COMMITHASH`

```bash
➜  webflyx git:(main) ✗ git switch -c update_dune d82806742765501b11e5226478e992b34743fbce

Switched to a new branch 'update_dune'

	➜  webflyx git:(update_dune) git log --oneline -n 1
d828067 (HEAD -> update_dune) D: added_classics
```

- To use rebase to bring changes from main onto a current branch (let's pretend we're on one called `jdsl`), we would run this while on the `jdsl` branch:
	- `git rebase main`
- This will do the following:
	- Identify the latest commit from main and use it as the temporary new base for the rebase process
	- Replay each commit from `jdsl` one at a time onto this temporary location
	- Update the `jdsl` branch to point to the last replayed commit in the temporary location, making this the new permanent `jdsl`.
	- The rebase does not affect the main branch; `jdsl` now includes all changes from main.

```bash
➜  webflyx git:(update_dune) ✗ git add quotes/dune.md
➜  webflyx git:(update_dune) ✗ git commit -m "H: First dune.md updation"
[update_dune 7590cf9] H: First dune.md updation
 1 file changed, 2 insertions(+)
➜  webflyx git:(update_dune) nvim .
➜  webflyx git:(update_dune) ✗ git add quotes/dune.md
➜  webflyx git:(update_dune) ✗ git commit -m "I: Second dune.md updation"
[update_dune 07293c4] I: Second dune.md updation
 1 file changed, 1 insertion(+), 1 deletion(-)
➜  webflyx git:(update_dune) git rebase main
Successfully rebased and updated refs/heads/update_dune.
➜  webflyx git:(update_dune) git log --oneline

643c22e (HEAD -> update_dune) I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles, main) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```

- Even though we originally branched off the `D` commit from `main`, we see a nice linear history from `A` to `I`.
## When to Rebase
- Git rebase and git merge are different tools.
- Merge
	- An advantage of merge is that it preserves the true history of the project.
	- It shows when branches were merged and where. 
	- One disadvantage is that it can create a lot of merge commits, which can make the history harder to read and understand.
- A linear history is generally easier to read, understand, and work with. 
- Warning
	- You should never rebase a public branch (like `main`) onto anything else.
	- Other developers have it checked out, and if you change its history, you'll cause a lot of problems for them.
	- However, with your own branch, you can rebase onto other branches (including a public branch like `main`) as much as you want.
# Reset
- One of the major benefits of using Git is the ability to undo changes. 
- There are a lot of different ways to do this
- First, by going back in the commit history without discarding changes.

```bash
➜  webflyx git:(update_dune) echo "* The Internship" > titles.md
➜  webflyx git:(update_dune) ✗ git status
On branch update_dune
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   titles.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  webflyx git:(update_dune) ✗ git add titles.md
➜  webflyx git:(update_dune) ✗ git commit -m "J: updated titles.md"
[update_dune 73d480d] J: updated titles.md
 1 file changed, 1 insertion(+), 8 deletions(-)
➜  webflyx git:(update_dune) git log --oneline

73d480d (HEAD -> update_dune) J: updated titles.md
643c22e I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles, main) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```

## Git Reset Soft
- The git reset command can be used to undo the last commit(s) or any changes in the index (staged but not committed changes) and the worktree (unstaged and not committed changes).
- `git reset --soft COMMITHASH`
- The `--soft` option is useful if you just want to go back to a previous commit, but keep all of your changes. 
- Committed changes will be uncommitted and staged, while uncommitted changes will remain staged or unstaged as before.

```bash
➜  webflyx git:(update_dune) git log
➜  webflyx git:(update_dune) commit 643c22ef0b7c131ab051a078c697b02d95cd3d80
➜  webflyx git:(update_dune) git reset --soft 643c22ef0b7c131ab051a078c697b02d95cd3d80
➜  webflyx git:(update_dune) ✗ git status
On branch update_dune
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   titles.md
        
➜  webflyx git:(update_dune) ✗ git log --oneline

643c22e (HEAD -> update_dune) I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles, main) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```
- It goes back to the `I` commit while keeping the changed `titles.md` (staged but not committed). `J` should be gone from the commit log.
## Git Reset Hard
- If we don't want to keep the changes to `titles.md`. 
- `git reset --hard COMMITHASH`
- The `--hard` flag makes your working directory and staging area match the specified commit exactly, discarding any local changes.
- This is useful if you just want to go back to a previous commit and discard all the changes.

```bash
➜  webflyx git:(update_dune) ✗ git log
➜  webflyx git:(update_dune) ✗ 643c22ef0b7c131ab051a078c697b02d95cd3d80
➜  webflyx git:(update_dune) ✗ git status
On branch update_dune
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   titles.md

➜  webflyx git:(update_dune) ✗ git reset --hard 643c22ef0b7c131ab051a078c697b02d95cd3d80
HEAD is now at 643c22e I: Second dune.md updation
➜  webflyx git:(update_dune) git status
On branch update_dune
nothing to commit, working tree clean
```
- If you were to simply delete a committed file, it would be trivially easy to recover because it is tracked in Git. However, if you used `git reset --hard` to undo committing that file, it would be deleted for good.
- If you want to reset back to a specific commit, you can use the git reset --hard command and provide a commit hash.
	- `git reset --hard a1b2c3d`
- This will reset your working directory and index to the state of that commit, and all the changes made after that commit are lost forever.

# Remote
- A Distributed Version Control System (DVCS) is a version control system in which every developer has a complete copy of the entire repository, including its full history.
- We can have "remotes," which are just external repos with mostly the same Git history as our local repo.
- When it comes to Git (the CLI tool), there really isn't a "central" repo. 
- GitHub is just someone else's repo. Only by convention and convenience have we, as developers, started to use GitHub as a "source of truth" for our code.
- In git, another repo is called a "remote." 
- The standard convention is that when you're treating the remote as the "authoritative source of truth" (such as GitHub) you would name it the "origin".
- "Authoritative source of truth" mean that it's the one you and your team treat as the "true" repo. It's the one that contains the most up-to-date version of the accepted code.
- `git remote add <name> <uri>`
- `➜  webflyx-local git:(main) git remote add origin ../webflyx`
## Fetch
- Adding a remote to our Git repo does not mean that we automagically have all the contents of the remote. 
- First, we need to fetch the contents.
- `git fetch`
- This downloads copies of all the contents of the `.git/objects` directory (and other bookkeeping information) from the remote repository into your current one. 

```bash
➜  webflyx-local git:(main) find .git/objects
.git/objects
.git/objects/pack
.git/objects/info
➜  webflyx-local git:(main) git fetch
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 30 (delta 10), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (30/30), 2.94 KiB | 1005.00 KiB/s, done.
From ../webflyx
 * [new branch]      main          -> origin/main
 * [new branch]      update_dune   -> origin/update_dune
 * [new branch]      update_titles -> origin/update_titles

➜  webflyx-local git:(main) find .git/objects
.git/objects
.git/objects/67
.git/objects/67/481ec9e7dc182c2ea63fef18f7f08b208a3e1e
.git/objects/18
.git/objects/18/d53b21a68cf7f8d8b4fb2c293ed409fb967cd3
.git/objects/14
....
....
....

➜  webflyx-local git:(main) git log
fatal: your current branch 'main' does not have any commits yet
```

- Just because we fetched all of the metadata from the remote webflyx repo doesn't mean we have all of the files.
## Log remote
- You can log the commits of a remote repo as well!
- `git log <remote>/<branch>`

```bash
➜  webflyx-local git:(main) git fetch origin
➜  webflyx-local git:(main) git log origin/update_dune --oneline
643c22e (origin/update_dune, origin/HEAD) I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (origin/update_titles, origin/main) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```
## Merge
- Just as we merged branches within a single local repo, we can also merge branches between local and remote repos.
- `git merge <remote>/<branch>`

```bash
➜  webflyx-local git:(main) git merge origin/main
➜  webflyx-local git:(main) git log --oneline

51597bf (HEAD -> main, origin/update_titles, origin/main) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```
# Github
- GitHub is the most popular website for Git repositories (projects) online.
- That is, for hosting "remotes" on a central website.
- GitHub serves several purposes:
	- As a backup of all your code on the cloud in case something happens to your computer
	- As a central place to share your code and collaborate on it with others
	- As a public portfolio for your coding projects
- Git and GitHub are not the same! 
- Git is an open-source command line tool for managing code files.
- GitHub and its primary competitors, GitLab and Bitbucket, are commercial web products that use Git. 
- Their websites give us a way to store our code that's managed by Git
## github cli
- `curl -sS https://webi.sh/gh | sh`
- `gh auth login`

```bash
➜  webflyx git:(update_dune) git remote add origin git@github.com:AbhinavGt/webflyx.git

➜  webflyx git:(update_dune) git ls-remote
Enter passphrase for key '/home/a8hi9t/.ssh/id_ed25519':
From git@github.com:AbhinavGt/webflyx.git
```
## git push
- The git push command pushes (sends) local changes to any "remote" - in our case, GitHub
- To push our local main branch's commits to the remote origin's main branch
	- `git push origin main`
- You need to be authenticated with the remote to push changes,
- You can also push a local branch to a remote with a different name:
	- `git push origin <localbranch>:<remotebranch>`
- You can also delete a remote branch by pushing an empty branch to it:
	- `git push origin :<remotebranch>`
```bash
➜  webflyx git:(update_dune) git switch main
Switched to branch 'main'
➜  webflyx git:(update_dune) git switch main
Switched to branch 'main'
➜  webflyx git:(main) git push origin main
Enter passphrase for key '/home/a8hi9t/.ssh/id_ed25519':
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 12 threads
Compressing objects: 100% (20/20), done.
Writing objects: 100% (22/22), 2.32 KiB | 2.32 MiB/s, done.
Total 22 (delta 6), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (6/6), done.
To github.com:AbhinavGt/webflyx.git
 * [new branch]      main -> main

```

  ![[Github-commits.png]]
## Pull
- when we want the actual file changes from a remote repo, not just the metadata.
- `git pull [<remote>/<branch>]`
- If you execute git pull without anything specified it will pull your current branch from the remote repo.

- edit contents.md and commit `J: update classics.csv`.
```bash
➜  webflyx git:(main) git config set pull.rebase false 
# make sure git will merge on a pull
➜  webflyx git:(main) git merge update_dune
Updating 51597bf..643c22e
Fast-forward
 quotes/dune.md | 2 ++
 1 file changed, 2 insertions(+)
➜  webflyx git:(main) git pull origin main
Enter passphrase for key '/home/a8hi9t/.ssh/id_ed25519':
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 971 bytes | 971.00 KiB/s, done.
From github.com:AbhinavGt/webflyx
 * branch            main       -> FETCH_HEAD
   51597bf..a886be7  main       -> origin/main
Merge made by the 'ort' strategy.
 classics.csv | 1 +
 1 file changed, 1 insertion(+)

➜  webflyx git:(main) git log --oneline

0ccb743 (HEAD -> main) K: Pulled from origin
a886be7 (origin/main) J: update classics.csv
643c22e (update_dune) I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md 
```

![[Git-pull.png|500]]

## Pull requests

- On GitHub, a pull request is a way to propose changes, typically to the rest of your team, or to the maintainer of a project you're contributing to.
- Pull requests allow team members to see what changes are being proposed and to discuss them before they are merged into the main codebase avoiding mess.

```bash
➜  webflyx git:(main) git branch -d update_dune
Deleted branch update_dune (was 643c22e).
➜  webflyx git:(main) git switch -c add_classics
Switched to a new branch 'add_classics'
➜  webflyx git:(add_classics) nvim .
➜  webflyx git:(add_classics) ✗ git add classics.csv
➜  webflyx git:(add_classics) ✗ git commit -m "L: Update classics.csv"
[add_classics 080f0c9] L: Update classics.csv
 1 file changed, 1 insertion(+)
➜  webflyx git:(add_classics) git push origin add_classics

Enter passphrase for key '/home/a8hi9t/.ssh/id_ed25519':
Enumerating objects: 21, done.
Counting objects: 100% (19/19), done.
Delta compression using up to 12 threads
Compressing objects: 100% (13/13), done.
Writing objects: 100% (13/13), 1.32 KiB | 1.32 MiB/s, done.
Total 13 (delta 6), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (6/6), completed with 2 local objects.
remote:
remote: Create a pull request for 'add_classics' on GitHub by visiting:
remote:      https://github.com/AbhinavGt/webflyx/pull/new/add_classics
remote:
To github.com:AbhinavGt/webflyx.git
 * [new branch]      add_classics -> add_classics
```

![[Git-pullReq.png]]

## Merge Pull Request
- In a typical team workflow, you would ask a team mate to review your pull request.
- If they approve of the changes, they would approve the pull request, and you'd be clear to merge.

![[Git-merge-pullReq.png]]

```bash
➜  webflyx git:(add_classics) git switch main
Switched to branch 'main'
➜  webflyx git:(main) git pull origin main
Enter passphrase for key '/home/a8hi9t/.ssh/id_ed25519':
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 908 bytes | 908.00 KiB/s, done.
From github.com:AbhinavGt/webflyx
 * branch            main       -> FETCH_HEAD
   a886be7..6a6d3ae  main       -> origin/main
Updating 0ccb743..6a6d3ae
Fast-forward
 classics.csv | 1 +
 1 file changed, 1 insertion(+)
➜  webflyx git:(main) git branch -d add_classics
Deleted branch add_classics (was 080f0c9).

➜  webflyx git:(main) git log --oneline
 
6a6d3ae (HEAD -> main, origin/main) Merge pull request #1 from AbhinavGt/add_classics
080f0c9 (origin/add_classics) L: Update classics.csv
0ccb743 K: Pulled from origin
a886be7 J: update classics.csv
643c22e I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```
# Gitignore
- If you work with Python, you probably want to ignore automatically generated files like `.pyc` and `pycache`. If you are building a server, you probably want to ignore `.env` files that might hold private keys. If you (work with JavaScript, you might want to ignore the `node_modules` directory.
- A `.gitignore` file solves this.
- A `.gitignore` file with `node_modules, .env`; Will ignore every path matching `node_modules` as a path segment (directory name or file name). 
- It ignores:
	- `node_modules/code.js`
	- `src/node_modules/code.js`
	- `src/node_modules`
- It does not ignore:
	- `src/node_modules_2/code.js`
	- `env/node_modules_3`
	- `src/node_modules.js`
- This will also ignore the .env file preventing you from committing sensitive environment variables (like API keys, DB credentials, etc.) ...cause that would be bad.

```bash
➜  webflyx git:(main) ✗ ls
classics.csv  contents.md  guilty_pleasures.md  quotes  secure  titles.md
➜  webflyx git:(main) ✗ cat .gitignore
secure
guilty_pleasures.md
➜  webflyx git:(main) ✗ git add .
➜  webflyx git:(main) ✗ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitignore
# not tracking secure and guilty_pleasures.md
➜  webflyx git:(main) ✗ git commit -m "M: Added gitignore"
[main bef4221] M: Added gitignore
 1 file changed, 2 insertions(+)
 create mode 100644 .gitignore
➜  webflyx git:(main) git log --oneline
 
bef4221 (HEAD -> main) M: Added gitignore
6a6d3ae (origin/main) Merge pull request #1 from AbhinavGt/add_classics
080f0c9 (origin/add_classics) L: Update classics.csv
0ccb743 K: Pulled from origin
a886be7 J: update classics.csv
643c22e I: Second dune.md updation
f37f5ff H: First dune.md updation
51597bf (update_titles) G: Updated titles.md
67481ec F: Merge branch 'add_classics'
0ed790c E: added contents
103b051 D: branching to add_classics
737bcdc C: add quotes
c8997d4 B: add titles
16b6ce3 A: add contents.md
```
## Nested Gitignore
- It's fairly common to have multiple `.gitignore` files in different directories throughout a project.
- A nested `.gitignore` file only applies to the directory it's in and its subdirectories.

## Patterns
### Wildcards

- The `*` character matches any number of characters except for a slash `/`.
- For example, to ignore all `.txt` files, you could use the following pattern:
	- `*.txt`
- This would ignore files like `/princess_diaries.txt` and `/contacts/your_mom.txt` since they're both `.txt` files.

### Rooted Patterns
- Patterns starting with a `/` are anchored to the directory containing the `.gitignore` file.
- For example:
	- `/main.py`
- This ignores `/main.py` but not `/src/main.py` since `/src` is a subdirectory.

### Negation
- You can negate a pattern by prefixing it with an exclamation mark `!`. 
- For example:
	- `*.txt !/important.txt`
- This would not ignore `/important.txt`, but would ignore `/self_affirmations/important.txt`.
- It will ignore all `.txt` files except for `important.txt`.
### Comments
- You can add comments to your `.gitignore` file by starting a line with a `#`. 
- For example:
	- `# Ignore all .txt files *.txt`
- Comments are especially helpful when doing something unconventional or complex, especially when collaborating
### Order Matters
- The order of patterns in a `.gitignore` file determines their effect, and patterns can override each other. 
- For example:
	- `temp/* !temp/instructions.md`
- Everything in the `temp/` directory would be ignored except for `instructions.md`. 
- If the order were reversed, `instructions.md` would be ignored.
## What to Ignore
- Ignore things that can be _generated_ (e.g. compiled code, minified files, etc.)
- Ignore dependencies (e.g. `node_modules`, `venv`, `packages`, etc.)
- Ignore things that are personal or specific to how you like to work (e.g. editor settings)
- Ignore things that are sensitive or dangerous (e.g. `.env` files, passwords, API keys, etc.)
```bash
➜  webflyx git:(main) ✗ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   .gitignore
        new file:   advert.md

➜  webflyx git:(main) ✗ git commit -m  "N: Added advert.md"
[main 9d5f6eb] N: Added advert.md
 2 files changed, 8 insertions(+)
 create mode 100644 advert.md
```
- If you accidentally staged or committed `advert.html`, adding it to the `.gitignore` file won't remove it from the repository. 
- You'll need to remove it manually (at which point it will continue to be ignored going forward):
	- `git rm --cached advert.html`