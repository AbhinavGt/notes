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
- The `unset` subcommand is used to _remove_ a configuration value.
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
- The first is `--decorate`. It can be one of:
	- `short` (the default)
	- `full` (shows the full ref name)
	- `no` (no decoration)
- A ref is just a pointer to a commit. All branches are refs, but not all refs are branches.
- `--oneline` flag will show you a more compact view of the log

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