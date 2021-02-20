---
title: Git学习笔记
date: 2021-02-19 22:16:53
tags:
	- Git
	- 代码管理
categories:
	- [工具, 版本控制]
---

[toc]

<!-- toc -->



# Git Book Notes

Link to the book: https://git-scm.com/book/en/v2



## Chapter 1 Getting Started

### 1.1. Three Main States

* ==committed==--means that the data is safely stored in your local database.
* ==modified==--means that you have changed the file but have not committed it to your database yet.
* ==staged==--means that you have marked a modified file in its current version to go into your next commit snapshot.

![Working tree, staging area, and Git directory.](https://git-scm.com/book/en/v2/images/areas.png)

The basic Git workflow goes something like this:

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds *only* those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

If a particular version of a file is in the Git directory, it’s considered *committed*. If it has been modified and was added to the staging area, it is *staged*. And if it was changed since it was checked out but has not been staged, it is *modified*. 



<!--more-->



### 1.2 First-Time Git Setup

#### 1.2.1 About configuration levels

`git config` tool can set/==get== configuration variables.

Variables can be stored in three different places:

1. ==`/etc/gitconfig`== file: Contains values applied to every user on the system and all their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically. (Because this is a system configuration file, you would need administrative or superuser privilege to make changes to it.)
2. ==`~/.gitconfig`== or ==`~/.config/git/config`== file. Values specific personally to you, the user. You can make Git read and write to this file specifically by passing the `--global` option, and this affects *all* of the repositories you work with on your system.
3. ==`config`== file in the Git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository. You can force Git to read from and write to this file with the `--local` option, but that is in fact the default. (Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.) 

To be summarized:

* ==system level==
* ==user level==
* ==directory level==

To view all of the settings and where they are coming from, using:

```shell
git config --list --show-origin
```

#### 1.2.2 Setup my identity info

```shell
git config --global user.name "movic"
git config --global user.email xxxxx.xxxx@xxxx.com
```

#### 1.2.3 Setup my editor

```shell
git config --global core.editor vim
# On a Windows system, if you want to use a different text editor, you must specify the full path to its executable file
```

#### 1.2.4 Setup credential cache

If you don’t want to type the password every single time you push, you can set up a “credential cache”. 

```shell
# In Git Book
git config --global credential.helper cahche
# But I found
git config --global credential.helper manager
```

## Chapter 2 Git Basics

### 2.1 Getting a Git Repository

#### 2.1.1 Initializing a repository in an existing directory

```shell
git init
```

then add file to be tracked by:

```shell
# if it’s a directory, the command adds all the files in that directory recursively
git add LICENSE
# or
git add *.py
# then
git commit -m "message of the commit."
```

#### 2.1.2 Clone an existing repository

```shell
git clone https://github.com/**/* [my_working_directory(optional)]
```

### 2.2 Recording Changes to the Repository

#### 2.2.1 Lifecycle of the status of the files

![The lifecycle of the status of your files.](https://git-scm.com/book/en/v2/images/lifecycle.png)

#### 2.2.2 Checking the status of the files

```shell
git status
## for short status
git status -s
# or
git status --short
```

==??== stands for untracked file, ==A== stands for added file, ==" M"== stands for modified but not yet staged file, =="M "== stands for modified and staged file, and ==MM== stands for modified, staged file, and then modified again.

#### 2.2.3 Staging modified files

```shell
# `git add` is a multipurpose command, it can be used to stage files as well
# staging your files
git add *.py
# if you already staged a file and then change it again, you need to stage your latest version of your file, specifically use the `git add` command
# anyway `git add` is all you need
```

#### 2.2.4 Ignoring files

The rules for the patterns you can put in the `.gitignore` file are as follows:

- Blank lines or lines starting with `#` are ignored.
- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
- You can start patterns with a forward slash (`/`) to avoid recursivity.
- You can end patterns with a forward slash (`/`) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (`!`).

Glob patterns are like simplified regular expressions that shells use. An asterisk (`*`) matches zero or more characters; `[abc]` matches any character inside the brackets (in this case a, b, or c); a question mark (`?`) matches a single character; and brackets enclosing characters separated by a hyphen (`[0-9]`) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`, and so on.

```shell
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

Tip: [Examples](https://github.com/github/gitignore) of `.gitignore` file.

#### 2.2.5 Viewing the staged and unstaged changes

```shell
# the below command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged
git diff
# to see what you’ve staged that will go into your next commit
git diff --staged
# or
git diff --cached
```

#### 2.2.6 Committing your changes

```shell
git commit -m message -v
```

#### 2.2.7 Skipping the staging area

```shell
# adding -a option to the `git commit` command will automatically add the tracked files to the staged area
git commit -a
```

#### 2.2.8 Removing files

```shell
# to remove a file from Git, use `git rm` command. This command will remove the file from working directory as well
git rm *.py
## also you can remove the file from the working directory followed by the `git rm` command
rm *.py
git rm *.py
# if you want to keep the file in your working tree but remove it from your staging area, use `--cached` option
git rm --cached *.py
# if you modified the file or had already added it to the staging area, you must force the removal with the -f option. This is a safety feature to prevent accidental removal of data that hasn’t yet been recorded in a snapshot and that can’t be recovered from Git
# we can also use glob pattern, note the backslash (\) in front of the *. This is necessary because Git does its own filename expansion in addition to your shell’s filename expansion
git rm log/\*.log
```

#### 2.2.9 Moving files

```shell
git mv a.py b.py
# or
mv a.py b.py
git rm a.py
git add b.py
```

### 2.3 Viewing the Commit History

#### 2.3.1 Options for formatting the log output

```shell
git log
# or with diff info, use `git log -p` or `git log --patch` followed by optional a `-number`, for example: -2
git log -p -2
# to print out summary of the commit or say, the statistics of the commit, use `--stat` option
git log --stat
# to format the log output, use `--pretty` option, A few prebuilt options are available
git log --pretty=oneline  # prints each commit on a single line
git log --pretty=short  # ignore the date
git log --pretty=full # show committer info compared with `short`
git log --pretty=fuller # show date info compared with `full`

# The `first..latter` syntax is a log filter that asks Git to display only those commits that are on the `latter` branch that are not on the `first` branch
# use `--no-merges` to prevent the display of merge commits
git log --no-merges first..latter
```

Customized log output —— ==`format`==, the `oneline` option and `format` option are particularly useful with another option called `--graph`

```shell
git log --pretty=format:"%h - % an" --graph
```

Useful options for `git log --pretty=format`:

| Option |               Description of Output               |
| :----: | :-----------------------------------------------: |
|  `%H`  |                    Commit hash                    |
|  `%h`  |              Abbreviated commit hash              |
|  `%T`  |                     Tree hash                     |
|  `%t`  |               Abbreviated tree hash               |
|  `%P`  |                   Parent hashes                   |
|  `%p`  |             Abbreviated parent hashes             |
| `%an`  |                    Author name                    |
| `%ae`  |                   Author email                    |
| `%ad`  | Author date (format respects the `--date=option`) |
| `%ar`  |               Author date, relative               |
| `%cn`  |                  Committer name                   |
| `%ce`  |                  Committer email                  |
| `%cd`  |                  Committer date                   |
| `%cr`  |             Committer date, relative              |
|  `%s`  |                      Subject                      |

A few common options listed below:

|      Option       |                         Description                          |
| :---------------: | :----------------------------------------------------------: |
|       `-p`        |         Show the patch introduced with each commit.          |
|     `--stat`      |      Show statistics for files modified in each commit.      |
|   `--shortstat`   | Display only the changed/insertions/deletions line from the --stat command. |
|   `--name-only`   | Show the list of files modified after the commit information. |
|  `--name-status`  | Show the list of files affected with added/modified/deleted information as well. |
| `--abbrev-commit` | Show only the first few characters of the SHA-1 checksum instead of all 40. |
| `--relative-date` | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format. |
|     `--graph`     | Display an ASCII graph of the branch and merge history beside the log output. |
|    `--pretty`     | Show commits in an alternate format. Options include `oneline`, `short`, `full`, `fuller`, and `format` (where you specify your own format). |
|    `--oneline`    | Shorthand for `--pretty=oneline --abbrev-commit` used together. |

 #### 2.3.2 Limiting log output

* ==Time filter==

```shell
# gets the list of commits made in last two weeks:
git log --since=2.weeks
# or
git log --since="2019-06-01"
# or
git log --since="2 years 1 day 3 minutes ago"

# until option
git log --until="now"
```

* ==Author filter==

```shell
git log --author="movic"
```

* ==Message filter==

```shell
# list out the commits that match the `keywords` in the message
git log --grep "version"
# to limit the ouput to match all --grep patterns, use `--all-match` option
git log --grep "version" --grep "2" --all-match
```

* ==Changes filter==

```shell
# this only list out the commits which add or remove the `keywords`
git log -S "hello world"
```

* ==Path filter==

```shell
# limit log output to commits that introduced a change to the specified files
git log -- models/
# or
git log -- main.py
```

|        Option         |                         Description                          |
| :-------------------: | :----------------------------------------------------------: |
|        `-<n>`         |                 Show only the last n commits                 |
| `--since`, `--after`  |  Limit the commits to those made after the specified date.   |
| `--until`, `--before` |  Limit the commits to those made before the specified date.  |
|      `--author`       | Only show commits in which the author entry matches the specified string. |
|     `--committer`     | Only show commits in which the committer entry matches the specified string. |
|       `--grep`        | Only show commits with a commit message containing the string |
|         `-S`          | Only show commits adding or removing code matching the string |

### 2.4 Undoing Things

#### 2.4.1 Modify a commit

* Change the commit message

```shell
git commit --amend -m "message"
```

* Add the forgotten file to the commit as well as change the commit message

```shell
#  the second commit replaces the results of the first
git commit -m "initial commit"
git add forgotten_file
git commit --amend -m "correct commit"
```

#### 2.4.2 Unstaging a staged file

```shell
# the file in the working directory is not touched
git reset HEAD -- <file>
```

#### 2.4.3 Unmodifying a modified file

```shell
# revert the file from last commit. Be careful, your modification will disappear
git checkout <file>
```

### 2.5 Work with Remotes

#### 2.5.1 Showing your remotes

To see which remote servers you have configured, run `git remote` command. The name `origin` is the default name Git gives to the server you cloned from.

```shell
git remote
# to show URLs
git remote -v
```

#### 2.5.2 Adding remote repositories

To add a new remote Git repository as a shortname you can reference easily, run `git remote add <shortname> <url>`

```shell
git remote add origin https://github.com/xxxxx/xxxxxx.git
```

#### 2.5.3 Fetching and pulling from your remote

```shell
# use `git fetch <remote>` to pull down all the data from that remote project, this doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready
git fetch origin

# use `git pull <remote>` to automatically fetch and then merge that remote branch into your current branch
git pull origin
```

#### 2.5.4 Pushing to your remotes

```shell
# back up to the server
git push origin master
```

#### 2.5.5 Inspecting a remote

Use the `git remote show <remote>` command to show more information about a particular remote.

```shell
git remote show origin
```

#### 2.5.6 Renaming and removing remotes

Run `git remote rename` to change a remote's shortname. This changes all your remote-tracking branch names too. Run `git remote remove` or `git remote rm` to remove a shortname.

```shell
# rename
git remote rename origin my_remote
# remove
git remote remove origin
```

### 2.6 Tagging

#### 2.6.1 Listing your tags

Just type `git tag` (with optional `-l` or `--list`)

```shell
git tag
# to search for tags that match a particular pattern, `-l` or `--list` is mandatory
git tag -l "v1.8.5*"
```

#### 2.6.2 Creating tags

* ==lightweight tag== -- a pointer to a specific commit

```shell
# to create a lightweight tag, don't supply any of the `-a`, `-s` or `-m` options, just provide a tag name
git tag v2.0
```

* ==annotated tag== -- full object in the Git database

```shell
# specify `-a` to create a annotated tag
git tag -a v2.0 -m "version 2.0"
# show the tagger infomation
git show v2.0
```

#### 2.6.3 Tagging later

```shell
# specify the commit checksum(or part of it) at the end of the command to create a tag for the past commit
git tag -a v2.0 9fceb02
```

#### 2.6.4 Sharing tags

By default, the `git push` command doesn't transfer tags to the remote servers. Explicitly push tags to a shared server by running `git push origin <tagname>`.

```shell
git push origin v2.0
# to transfer all the tags, use `--tags` option
git push origin --tags
# when someone else clones or pulls from your repository now, they will get all your tags as well
```

#### 2.6.5 Deleting tags

Use `git tag -d <tagname>` to delete a tag on your local repository.

```shell
# note that this does not remove the tag from any remote servers
git tag -d v2.0
# to delete a tag from a remote server, use `git push <remote> :refs/tags/<tagname>`
git push origin :refs/tags/v2.0
# or
git push origin --delete v2.0
```

#### 2.6.6 Checking out tags

This puts your repository in ==detached HEAD== state, which has some ==ill side effects==.

```shell
git checkout v2.0
```

> In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:

```shell
git checkout -b version2 v2.0.0
# if you do this and make a commit, your `version2` branch will be slightly different than the `v2.0.0` tag since it will move forward with your new changes, so do be careful
```

### 2.7 Git Aliases

#### 2.7.1 Setup aliases

```shell
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'

# incorporate with external command
git config --global alias.imshow '!display'
```

## Chapter 3 Git Branching

### 3.1 Branches in a Nutshell

When you make a commit, Git stores a commit object that contains a ==pointer== to the snapshot of the content you staged. This object also contains the ==metadata==, including the author's name and email address, the message and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one for a normal commit, and multiple for a commit that results from a merge of two or more branches. 

Some objects after a commit:

1. *blobs*
2. *tree*
3. *commit*

![A commit and its tree.](https://git-scm.com/book/en/v2/images/commit-and-tree.png)

![Commits and their parents.](https://git-scm.com/book/en/v2/images/commits-and-parents.png)

#### 3.1.1 Creating a new branch

Use `git branch` command to create a new branch.

```shell
git branch testing
# this creates a new pointer to the same commit you're currently on
```

![A branch and its commit history.](https://git-scm.com/book/en/v2/images/branch-and-history.png)

![Two branches pointing into the same series of commits.](https://git-scm.com/book/en/v2/images/two-branches.png)

`HEAD` is a special pointer, which points to the current commit. The `git branch` command only created a new branch -- it didn't switch to that branch. To checkout a branch at the same time, run `git checkout -b <newbranchname>` command

![HEAD pointing to a branch.](https://git-scm.com/book/en/v2/images/head-to-master.png)

Run `git log --decorate` to show the pointed commit of `HEAD` pointer 

```shell
git log --decorate
```

#### 3.1.2 Switch branches

Run `git checkout` to switch to a existing branch.

```shell
git checkout testing
```

![HEAD points to the current branch.](https://git-scm.com/book/en/v2/images/head-to-testing.png)

If we make a new commit on the `testing` branch, then the `testing` branch will move forward.

![The HEAD branch moves forward when a commit is made.](https://git-scm.com/book/en/v2/images/advance-testing.png)

And run `git checkout master` to switch back.

![HEAD moves when you checkout.](https://git-scm.com/book/en/v2/images/checkout-master.png)

Now if you make a new commit on the master branch.

![Divergent history.](https://git-scm.com/book/en/v2/images/advance-master.png)

Use `git log --all` to print out ==all== the commits

### 3.2 Basic Branching and Merging

#### 3.2.1 Basic branching workflow

1. Do some work on a website
2. Create a branch for a new user story you're working on
3. Do some work in that branch

At this stage, you'll receive a call that another issue is critical and you need a hotfix. You'll do the following:

1. Switch to your production branch
2. Create a branch to add the hotfix
3. After it's tested, merge the hotfix branch, and push to production
4. Switch back to your original user story and continue working

#### 3.2.2 Basic branching

1. current state

![A simple commit history.](https://git-scm.com/book/en/v2/images/basic-branching-1.png)

2. create a `iss53` branch

![Creating a new branch pointer.](https://git-scm.com/book/en/v2/images/basic-branching-2.png)

3. make a new commit on branch `iss53`

![The `iss53` branch has moved forward with your work.](https://git-scm.com/book/en/v2/images/basic-branching-3.png)

4. switch back to your `master` branch, and create a hotfix branch to fix the bug

![Hotfix branch based on `master`.](https://git-scm.com/book/en/v2/images/basic-branching-4.png)

5. run some tests on branch `hotfix`, and use `git merge` command to merge branch `hotfix` to the master branch when you're ready

```shell
git checkout master
git merge hotfix
# when you try to merge one commit with a commit that can be reached by following the first commit's history, Git simplifies things by moving the pointer forward because there is no divergent work to merge together, this is called a "fast-forward"
```

![`master` is fast-forwarded to `hotfix`.](https://git-scm.com/book/en/v2/images/basic-branching-5.png)

6. run `git branch -d <branchname>` to delete the hotfix branch

```shell
git branch -d hotfix
# use `git branch -D` command to force delete a not merged branch
git branch -D hotfix
```

7. back to your original work, the ==work-in-progress== branch`iss53`

![Work continues on `iss53`.](https://git-scm.com/book/en/v2/images/basic-branching-6.png)

#### 3.2.3 Basic merging

All you have to do is check out the branch you wish to merge into and then run `git merge <branchname>` command.

Now if you are ready to merge you `iss53` branch to the `master` branch, you'll have some subtlety.  If the commit on the branch you're on isn't a direct ancestor of the branch you're merging in,  Git will do a ==three-way== merge. In this case, Git will use the snapshots pointed to by the branch tips (here, master and iss53) and the common ancestor of the two. Git creates a new snapshot that results from the branch tips and the ancestor, and automatically creates a new commit, which is called a ==merge commit==.

![Three snapshots used in a typical merge.](https://git-scm.com/book/en/v2/images/basic-merging-1.png)

![A merge commit.](https://git-scm.com/book/en/v2/images/basic-merging-2.png)

#### 3.2.4 Basic merge conflicts

If you changed the same part of the same file differently in the two branches you're merging, you'll get a merge conflict.

```shell
# To see the unmerged file, run `git status` command
git status
```

```shell
# Open the unmerged file, and select the section of either the HEAD branch or the branch you're merging
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

```shell
# Suppose you choose the iss53
<div id="footer">
please contact us at email.support@github.com
</div>
```

```shell
# Run `git add` on each file to mark it as resolved
git add index.html
```

```shell
# Another way is to use a graphical tool to resolve the merge conflicts
git mergetool
```

````shell
# After that, run `git status` to verify that all conflicts have been resolved
git status
# Finally, commit your merge.
git commit -m "merge commit"
````

### 3.3 Branch Management

To see the last commit on each branch, run `git branch -v`

```shell
git branch -v
```

The useful `--merged` and `--no-merged` options can filter this list to branches that you have or have not yet merged into the branch you're currently on. Also, you can specify a branch name to replace the default ==current branch==

```shell
git branch --merged <branchname>
git branch --no-merged <branchname>
```

 ### 3.4 Branching Workflows

#### 3.4.1 Long-Running branches

The stable branches are farther down the line in your commit history, and the bleeding-edge branches are farther up the history.

![A linear view of progressive-stability branching.](https://git-scm.com/book/en/v2/images/lr-branches-1.png)

![A ``silo'' view of progressive-stability branching.](https://git-scm.com/book/en/v2/images/lr-branches-2.png)

#### 3.4.2 Topic branches

Topic branch is a ==short-lived== branch for a single particular feature or related work.

Consider an example below:

![Multiple topic branches.](https://git-scm.com/book/en/v2/images/topic-branches-1.png)

Ignore the `iss91` branch and merge both `dumbidea` and `iss91v2` into the `master` branch.

![History after merging `dumbidea` and `iss91v2`.](https://git-scm.com/book/en/v2/images/topic-branches-2.png)

### 3.5 Remote Branches

#### 3.5.1 Fetching

When you clone a repository from a server:

![Server and local repositories after cloning.](https://git-scm.com/book/en/v2/images/remote-branches-1.png)

As long as you stay out of contact with your origin server, your `origin/master` pointer doesn't move:

![Local and remote work can diverge.](https://git-scm.com/book/en/v2/images/remote-branches-2.png)

To synchronize your work with a given remote, you can run a `git fetch <remote>` command. This command looks up which server `origin` is, fetches any data from it that you don't yet have, and updates your local database, moving your `origin/master` pointer to its new, more up-to-date position.

![`git fetch` updates your remote references.](https://git-scm.com/book/en/v2/images/remote-branches-3.png)

To demonstrate having multiple remote servers and what remote branches for those remote projects look like, here we can add a new remote server `teamone`.

![Adding another server as a remote.](https://git-scm.com/book/en/v2/images/remote-branches-4.png)

Now you can run `git fetch teamone` to fetch everything the remote `teamone` server has that you don't have yet. Here Git fetches no data but sets a remote-tracking branch called `teamone/master` to point to the commit that `teamone` has its `master` branch.

​	![Remote tracking branch for `teamone/master`.](https://git-scm.com/book/en/v2/images/remote-branches-5.png)

It’s important to note that when you do a fetch that brings down new remote-tracking branches, you don’t automatically have local, editable copies of them. In other words, in this case, you don’t have a new `master` branch — you have only an `teamone/master` pointer that you can’t modify.

#### 3.5.2 Pushing

```shell
# git push <remote> <branch>
git push origin serverfix
# is equivalent to `refs/heads/serverfix:refs/heads/serverfix`, or
# git push <remote> <localbranch>:<remotebranch>
git push origin serverfix:awesomebranch
# Below is called a `refspec`. The `-u` flag is short for `--set-upstream` which configures the branches for easier pushing and pulling later
git push -u origin featureB:featureBee
```

To merge this work into your current working branch, you can run `git merge origin/serverfix`. If you want your own local branch of the remote master branch that you can work on, you can base it off your remote-tracking branch:

```shell
git checkout -b serverfix origin/serverfix
# which is equivalent to
git checkout --track origin/serverfix
# or even, if the branch name you're trying to checkout doesn't exist and exactly matches a name on only one remote, Git will create a tracking branch for you.
git checkout serverfix
```

#### 3.5.3 Tracking branches

Checking out a local branch from a remote-tracking branch automatically creates what is called a “tracking branch” (and the branch it tracks is called an “upstream branch”). 

```shell
# to change the upstream branch you're tracking, you can use `-u` or `--set-upstream-to` option to `git branch`
git branch -u origin/serverfix
```

To see what tracking branches you have set up, use `-vv` option to `git branch` command.

```shell
git branch -vv
```

#### 3.5.4 Pulling

Generally it's better to simply use the `fetch` and `merge` commands explicitly as the magic of `git pull` can often be confusing.

#### 3.5.5 Deleting remote branches

You can delete a remote branch using the `--delete` option to `git push`.

```shell
git push origin --delete serverfix
```

### 3.6 Rebasing

#### 3.6.1 The basic rebase

![Simple divergent history.](https://git-scm.com/book/en/v2/images/basic-rebase-1.png)

For three-way merging:

![Merging to integrate diverged work history.](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)

However, there is another way: you can take the patch of the change that was introduced in `C4` and reapply it on top of `C3`. In Git, this is called **rebasing**. With `rebase` command, you can take all the changes that were committed on one branch and replay them on a different branch.

```shell
# check out the `experiment` branch, and then rebase it onto the `master` branch
git checkout expriment
git rebase master
```

This operation works by going to the common ancestor of the two branches (the one you’re on and the one you’re rebasing onto), getting the diff introduced by each commit of the branch you’re on, saving those diffs to temporary files, resetting the current branch to the same commit as the branch you are rebasing onto, and finally applying each change in turn.

![Rebasing the change introduced in `C4` onto `C3`.](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)

Do a fast-forward merge

![Fast-forwarding the master branch.](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)

#### 3.6.2 More interesting rebases

Suppose you have a commit history like this:

![A history with a topic branch off another topic branch.](https://git-scm.com/book/en/v2/images/interesting-rebase-1.png)

You want to merge your client-side changes into your mainline for a release, but you want to hold off on the server-side changes until it's tested further. You can take the changes on client that aren't on server(`C8` and `C9`) and replay them on your `master` branch by using `--onto` option of `git rebase`

```shell
git rebase --onto master server client
```

This basically says, "Take the `client` branch, figure out the patches since it diverged from the `server` branch, and replay these patches in the `client` branch as if it was directly off the `master` branch instead."

![Rebasing a topic branch off another topic branch.](https://git-scm.com/book/en/v2/images/interesting-rebase-2.png)

```shell
# Do a Fast-forward Merge
git checkout master
git merge client
```

![Fast-forwarding your master branch to include the client branch changes.](https://git-scm.com/book/en/v2/images/interesting-rebase-3.png)

Now you want to pull in your `server` branch as well. You can rebase the server onto the `master` branch without having to check it out first by running `git rebase <basebranch> <topicbranch>` -- which checks out the topic branch for you and replays it onto the base branch.

```shell
git rebase master server
```

![Rebasing your server branch on top of your master branch.](https://git-scm.com/book/en/v2/images/interesting-rebase-4.png)

```shell
# Do a Fast-forward Merge
git checkout master
git merge server
# And delete the `client` branch and the `server` branch
git branch -d client
git branch -d server
```

![Final commit history.](https://git-scm.com/book/en/v2/images/interesting-rebase-5.png)

#### 3.6.3 The perils of rebasing

> Do not rebase commits that exist outside your repository and people may have based work on them.

Let's look at an example of how rebasing work that you're made public can cause problems. Suppose you clone from a central server and then do some work off  that. Your commit history looks like this:

![Clone a repository, and base some work on it.](https://git-scm.com/book/en/v2/images/perils-of-rebasing-1.png)

Now, someone else does more work that includes a merge, and pushes that work to the central server. You fetch it and merge the new remote branch into your work, making your history look something like this:

![Fetch more commits, and merge them into your work.](https://git-scm.com/book/en/v2/images/perils-of-rebasing-2.png)

Next, the person who pushed the merged work decides to go back and rebase their work instead; they do a `git push --force` to overwrite the history on the server. You then fetch from that server, bringing down the new commits.

![Someone pushes rebased commits, abandoning commits you’ve based your work on.](https://git-scm.com/book/en/v2/images/perils-of-rebasing-3.png)

If you do a `git pull` now, you'll create a merge commit which includes both lines of history, and your repository will look like this:

![You merge in the same work again into a new merge commit.](https://git-scm.com/book/en/v2/images/perils-of-rebasing-4.png)

The author originally wanted to remove the `C4` and `C6` commits, but if you push your work back to the server, you'll reintroduce all those commits to the central server.

####  3.6.4 Rebase when you rebase

If you do find yourself in a situation like the above one, i.e. if someone on your team force pushes changes that overwrite work that you've based work on, your challenge is to figure out what is yours and what they've rewritten.

It turns out that in addition to the commit SHA-1 checksum, Git also calculates a checksum that is based just on the patch introduced with the commit. This is called a “patch-id”.

For instance, in the previous scenario, if instead of doing a merge when we’re at Someone pushes rebased commits, abandoning commits you’ve based your work on we run `git rebase teamone/master`, Git will:

* Determine what work is unique to our branch (C2, C3, C4, C6, C7)
* Determine which are not merge commits (C2, C3, C4)
* Determine which have not been rewritten into the target branch (just C2 and C3, since C4 is the same patch as C4')
* Apply those commits to the top of `teamone/master`

![Rebase on top of force-pushed rebase work.](https://git-scm.com/book/en/v2/images/perils-of-rebasing-5.png)

```shell
git pull --rebase
# or
git fetch
git rebase teamone/master
```

#### 3.6.5 Rebase vs. Merge

Now that  you've seen rebasing and merging in action, you may be wondering which one is better. 

The answer is ==It's up to you==

> In general the way to get the best of both worlds is to rebase ==local changes== you've made but haven't shared yet before you push them in order to clean up your story, but ==never== rebase anything you've pushed somewhere.

## Chapter 5 Distributed Git

### 5.1 Distributed Workflows

#### 5.1.1 Centralized workflow

![Centralized workflow.](https://git-scm.com/book/en/v2/images/centralized_workflow.png)
**Note**: This workflow is the same as the one we've learnt above.

#### 5.1.2 Integration-Manager workflow

The process works as follows:

1. The project maintainer pushes to their public repository.
2. A contributor clones that repository and makes changes.
3. The contributor pushes to their own public copy.
4. The contributor sends the maintainer an email asking them to pull changes.
5. The maintainer adds the contributor's repository as a remote and merges locally.
6. The maintainer pushes merged changes to the main repository.

![Integration-manager workflow.](https://git-scm.com/book/en/v2/images/integration-manager.png) 

#### 5.1.3 Dictator and Lieutenants workflow

This is generally used by huge projects with hundreds of collaborators. Various integration managers are in charge of certain parts of the repository, they're called *lieutenants*. All the lieutenants have one integration manager known as the benevolent dictator. The process works like this:

1. Regular developers work on their topic branch and rebase their work on top of `master`. The `master` branch is that of the reference repository to which the dictator pushes.
2. Lieutenants merge the developer's topic branches into their `master` branch.
3. The dictator merges the lieutenants' `master` branches into the dictator's `master` branch.
4. Finally, the dictator pushes that `master` branch to the reference repository so the other developers can rebase on it.

![Benevolent dictator workflow.](https://git-scm.com/book/en/v2/images/benevolent-dictator.png)

### 5.2 Contributing to a Project

#### 5.2.1 Commit guidelines

1. Your submission should not contain any whitespace errors;

```shell
# before your commit, run `git diff --check` to identifies possible whitespace errors and lists them for you
git diff --check
```

2. Try to make each commit a logically separate changeset. ==At least one== commit per issue, with a useful message per commit. If some of the changes modify the same file, try to use `git add --patch` to ==partially stage files== (covered in detail in Interactive Staging). And this also makes it easier to ==revert== one of the changesets;
3. Commit message should start with a single line that's ==no more than about 50 characters== and that describes the changeset ==concisely==, `followed by` a ==blank line==, `followed by` a ==more detailed explanation==. Write your commit message in the imperative: "Fix bug" and not "Fixed bug" or "Fixes bug." Here is a [template originally written by Tim Pope](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) .

#### 5.2.2 Private small team

![General sequence of events for a simple multiple-developer Git workflow.](https://git-scm.com/book/en/v2/images/small-team-flow.png)

#### 5.2.3 Private managed team

![Basic sequence of this managed-team workflow.](https://git-scm.com/book/en/v2/images/managed-team-flow.png)

#### 5.2.4 Forked public project

One can contribute to a public project via:

1. forking
2. email

Basic contributing workflow:

1.  clone the main repository
2.  create a `topic` branch
3.  make some commits
4.  click the `fork` button
5.  add the forked repository URL as a new remote of your local repository
6.  push the `topic` branch you're working on to your forked repository
7.  send a *pull request*

```shell
# the `git request-pull` command takes the base branch into which you want your topic branch pulled and the Git repository URL you want them to pull from, and produces a summary of all the changes you're asking to be pulled
git request-pull origin/master myfork
```

Tips:

1. your local `master` branch always track `origin/master` branch, and to do your work in topic branches that you can easily discard if they're rejected. This makes you easier to rebase your work to the tip of the main repository.
2. if you want to submit a second topic of work to the project, don't continue working on the topic branch you just pushed up, start over from the main repository's `master` branch

```shell
git checkout -b featureB origin/master
```

Let's say the project maintainer has pulled in a bunch of other patches and tried your first branch, but it no longer cleanly merges. In this case, you can try to rebase that branch on top of `origin/master`， ==resolve== the conflicts for the maintainer, and then resubmit your changes:

```shell
git checkout featureA
git rebase origin/master
git push -f myfork featureA
```

Another scenario: the maintainer asks you to change an implementation detail of your topic, you can branch a new branch off the `origin/master` branch, and merge in your topic, change the implementation and push to `myfork` again.

```shell
git checkout -b featureBv2 origin/master
git merge --squash featureB
... change implementation ...
git commit
git push myfork featureBv2
```

Here, the `--squash` option takes all the work on the merged branch and squashes it into one changeset producing the repository state as if a real merge happened, without actually making a merge commit. This means your future commit will have one parent only and ==allows you to introduce all the changes from another branch and then make more changes before recoding the new commit==. Also, the `--no-commit` option can be useful to delay the merge commit in case of the default merge process.

### 5.3 Maintaining a Project

#### 5.3.1 Checking out remote branches

```shell
# got a remote URL from the contributor, add it as your remote repository
git remote add team_member url
git fetch team_member
git checkout -b topic team_member/topic
```

#### 5.3.2 Determining what is introduced

this section revisits some useful command and their variations.

```shell
# exclude commits between branches by add `--not` option before the branch name
git log contrib --not master
# which is equivalent to
git log master..contrib

# to see what changes each commit introduces, remember that you can pass the `-p` option to `git log` and it will append the diff introduced to each commit
git diff contrib --not master -p

# to see the changes added to the topic branch, do a `diff` between the last commit of the branch you're on and its common ancestor with another branch. This command shows you only the work your current topic branch has introduced since its common ancestor with master
git diff master...contrib
```

#### 5.3.3 Integrating contributed work

##### 5.3.3.1 merging workflows

1. `topic` branch -> `master` branch
2. `topic` branch -> `dev` branch > `master` branch
3. `topic` branch -> `integrate` branch -> `dev` branch > `master` branch

##### 5.3.3.2 large-merge workflows

##### 5.3.3.3 rebasing and Cherry-Picking workflows

1. rebase
2. cherry-pick

A ==cherry-pick== in Git is like a rebase for a ==single commit==. It takes the patch that was introduced in a commit and tries to reapply it ==on the branch you're currently on==. This is useful if you have ==a number of commits on a topic branch== and you want to integrate ==only one of them==, or if you ==only have one commit on a topic branch== and you'd prefer to cherry-pick it rather than run rebase.

```shell
git cherry-pick e43a6
```

![Example history before a cherry-pick.](https://git-scm.com/book/en/v2/images/rebasing-1.png)

![History after cherry-picking a commit on a topic branch.](https://git-scm.com/book/en/v2/images/rebasing-2.png)

#### 5.3.4 Rerere

Rerere stands for "reuse recorded resolution", it comes in two parts: ==a configuration== and ==a command==. It's a way of shortcutting manual conflict resolution.

```shell
git config --global rerere.enabled true
```

After enable `rerere`，whenever you do a merge that resolve conflicts, the resolution will be recorded in the cache in case you need it in the future. Run`git rerere` command when you're fixing merge conflicts, Git checks its database of resolutions and tries to find a match with any current merge conflicts and resolve them automatically.



#### 5.3.5 Tagging your releases

==Tag== -- You can recreate that release at any point, see [here](https://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project) for more details.

## Chapter 6 GitHub

### 6.1 Contributing to a Project

#### 6.1.1 The GitHub flow

1. Fork the project
2. Create a topic branch from `master`
3. Make some commits to improve the project
4. Push this branch to your GitHub project
5. Open a Pull Request on GitHub
6. Discuss, and optionally continue committing
7. The project owner merges or closes the Pull Request
8. Sync the uploaded master back to your fork

#### 6.1.2 Advanced pull requests

How to fix merge conflicts?

1. rebase your branch on top of whatever the target branch is (normally the `master` branch of the repository you forked)

2. merge the target branch (original repository) into your branch
   1. Add the original repository as a remote named "upstream"
   2. Fetch the newest work from that remote
   3. Merge the main branch of that repository into your topic branch
   4. Fix the conflict that occurred
   5. Push back up to the same topic branch

How do I reference the old Pull Request?

1. simply put `#<num>` in any comment or description within the repository

2. put `username#<num>` referring to an Issue or Pull Request in a fork
3. put `username/repo#<num>` referring to something in another repository
4. same way for SHA-1 info to replace `<num>` to refer to a commit

 ### 6.1.3 Keep your GitHub public repository up-to-date

1. If you were on another branch, return to `master`
2. Fetch changes from URL and merge them into `master`
3. Push your `master` branch to `origin`

```shell
git checkout master
git pull URL
git push origin master
```

Automate Configuration:

1. Add the source repository and give it a name.
2. Set your `master` branch to fetch from the URL remote
3. Define the default push repository to `origin`

```shell
git remote add name URL
git branch --set-upstream-to=name/master master
git config --local remote.pushDefault origin
```

Be Careful:

1. If you were on another branch, return to `master`
2. Fetch changes from URL and merge changes into `master`
3. Push your `master` branch to `origin`

```shell
git checkout master
git pull
git push
```

### 6.3 Maintaining a Project

#### 6.3.1 Managing pull requests

1. receive the email
2. iterate on Pull Requests page
3. pull request refs for merging in many Pull Requests branches, this requires configuration

#### 6.3.2 Pull requests on pull requests

#### 6.3.3 Contributing

`CONTRIBUTING` file is for specifying your criterion of contributors contribute to your repository. If this file exists, GitHub will show some guidelines to the contributors when they create a Pull Request.

## Chapter 7 Git Tools

### 7.1 Revision Selection

```shell
# use `git show` to examine the a specific commit
git show ca82939
git show topic
```

```shell
# show full SHA-1 hash, use `git rev-parse`
git rev-parse c32394d
git rev-parse dev
```

```shell
# similar to shell history, Git has `git reflog`
git reflog

# to see the fifth prior value of the HEAD of your repository, use @{5}
git show HEAD@{5}

# to see where your `master` branch was yesterday
git show master@{yesterday}

# to see reflog information formatted like the `git log` output, run `git log -g`
git log -g master
```

```shell
# to show the parent of HEAD, use `^` or `~`
git show HEAD^
# on Windows, use double `^^` or put the commit reference in quotes
git show HEAD^^
git show "HEAD^"
# you can also specify a number after `^` to identify which parent you want, for example, to see the second parent of `d921970` commit, for the case of more than one parent, especially the multi commit merge
git show d921970^2
```

```shell
# using either the `character` or `--not` before any reference from which you don't want to see reachable commits. The following three commands are equivalent:
git log refA..refB
git log ^refA refB
git log refB --not refA
# to see all commits that are reachable from `refA` or `refB` but not from `refC`, you can use either of:
git log refA refB ^refC
git log refA refB --not refC
```

```shell
# the triple-dot syntax, which specifies all the commits that are reachable by either of two references but not by both of them. To make the output more useful, pass `--left-right` option to the command
# result = union(set(master), set(experiment)) - intersection(set(master), set(experiment))
git log --left-right master...experiment
```

### 7.2 Interactive Staging

If you run `git add` with the `-i` or `--interactive` option, Git enters an interactive shell mode,  it lists the changes you've staged on the left and unstaged changes on the right.

```shell
# Some useful patch level commands:
git add -p
# or
git add --patch

git reset --patch

git checkout --patch
```

### 7.3 Stashing and Cleaning

#### 7.3.1 Stashing

To push a new stash onto your stack, run `git stash` or `git stash push`.

To see which stashes you've stored, you can use `git stash list`.

You can reapply the one you just stashed by using the `git stash apply` command, or if you want to apply one of the older stashes, you can specify it by naming it, like this: `git stash apply stash@{2}`. The changes to your files were reapplied, but the file you staged before wasn't restaged. To do that, you must run `git stash apply` command with a `--index` option to tell the command to try to reapply the staged changes.

To remove a stash from your stack, run `git stash drop` with the name of the stash. Alternatively, you can run `git stash pop` to apply the stash and then immediately drop it from your stack.

With the `--keep-index` option, Git will stash all the changes, but also leaved the staged file to the working directory. The unstaged file will be removed.

If you specify `--include-untracked` or `-u`, Git will include untracked files in the stash being created. However, including untracked files in the stash will still not include explicitly ignored files, which should be included by passing the `--all` or `-a` option.

To interactively stash patches, pass `--patch` option to the `git stash` command.

To create a branch from a stash, you can run `git stash branch <new branchname>`, which creates a new branch for you with your selected branch name, checks out the commit you were on when you stashed your work, reapplies your work there, and then drops the stash if it applies successfully.

#### 7.3.2 Cleaning

To clear all untracked files, use `git clean`. To remove all the untracked files in your working directory, you can run `git clean -f -d`, which removes any files and also any *subdirectories* that become empty as a result.

If you want to see what it would do, you can run the command with the `--dry-run` (or `-n`) option, which means "do a dry run and tell me what you would have removed."

To keep all the files instead, use `git stash --all`.

Add a `-x` option to the clean command to clear ignored files as well.

### 7.5 Searching

Use `git grep` command to search the given pattern. By default, `git grep` will look through the files in ==your working directory==. To print out the line numbers where Git has found matches, use `-n` or `--line-number` option.

```shell
git grep -n movic

# with `-c` or `--count` option to count the matches in each file
git grep -c movic

# you can display the enclosing method or function or class for each matching string with either of the `-p` or `--show-function` options:
git grep -p movic main.py

# search multiple pattern in the same line of text, use `--and` option
git grep -n -e "movic" --and -e "movie"

# have a line break after each file, use `--break`, group all the matches belong to the same file, use `--heading`
git grep -n --break --heading "movic" --and -e "movie"

# show the history of a function or line of code in your codebase
git log -L :{pattern}:file
# example
git log -L :movic:main.py
```

### 7.6 Rewriting History

> Don't push your work until you're happy with it
>
> Don't amend your last commit if you've already pushed it

To modify a commit that is farther back in your history, use `git rebase -i` tool.

```shell
git rebase -i HEAD~3
# edit the file in the editor, change the word `pick` to `edit` for every commit you want to amend at the begining of the file, and save & close it.
# example
# pick f7f3f6d changed my name a bit
# edit 310154e updated README formatting and added blame
# pick a5f4a0d added cat-file

# run `git commit --amend` to amend your commit
git commit --amend
# run `git rebase --continue` to continue amend later `edit` annotated commits
git rebase --continue

# To remove the commits or reorder the commits, simply remove the lines or reorder the lines in the editor mentioned above

# You can squash several commits to a single commit by specifying `squash` in the editor, Git applies both that change and the change directly before it and makes you merge the commit together
# example
# pick f7f3f6d changed my name a bit
# squash 310154e updated README formatting and added blame
# squash a5f4a0d added cat-file

# To Split a commit, specify `edit` of that commit in the editor, when you are at that commit, reset the staging area, and instead stage the changes multiple times as you expected, and finally run `git rebase --continue` to continue the rebase procedure
```

**The Nuclear Option: filter-branch**

```shell
git filter-branch --tree-filter "rm -f passwords.txt" HEAD
# to rewrite all branch, pass `--all` to the command

# The `--tree-filter` option runs the specified command after each checkout of the project and then recommits the results.

# To clear the backup generated by this command, run `git filter-branch -f`
git filter-branch -f


# Changing Email Address Globally
git filter-branch --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```

### 7.7 Reset Demystify

* The Head: your last commit on that branch
* The Index: your proposed next commit
* The Working Directory: working tree

#### 7.7.1 The role of reset

1. ==Move the branch HEAD== points to (stop here if `--soft`)， opposite op of commit
2. Make the index look like HEAD (stop here unless `hard`), default: `--mixed`
3. Make the working directory look like the index (be careful)

#### 7.7.2 Reset with a path

If you specify a path, `reset` will skip step 1, and limit the remainder of its actions to a specific file or set of files. It is the opposite operation of `git add`.

So you can directly pull content of arbitrary commit to the current index tree, without copying that content to the working directory before head.

#### 7.7.3 Squashing

Skip the intermediate commit, use `reset --soft` followed by `git commit`

![reset squash r1](https://git-scm.com/book/en/v2/images/reset-squash-r1.png)

![reset squash r2](https://git-scm.com/book/en/v2/images/reset-squash-r2.png)

![reset squash r3](https://git-scm.com/book/en/v2/images/reset-squash-r3.png)

#### 7.7.4 Check it out

Checkout with path will overwrites the file in the working directory. It would be exactly like `git reset --hard [branch] file`, it does not move HEAD.

#### 7.7.5 Cheat-sheet

|                             | HEAD | Index | Workdir | WD Safe? |
| :-------------------------- | :--- | :---- | :------ | -------- |
| **Commit Level**            |      |       |         |          |
| `reset --soft [commit]`     | REF  | NO    | NO      | YES      |
| `reset [commit]`            | REF  | YES   | NO      | YES      |
| `reset --hard [commit]`     | REF  | YES   | YES     | **NO**   |
| `checkout <commit>`         | HEAD | YES   | YES     | YES      |
| **File Level**              |      |       |         |          |
| `reset [commit] <paths>`    | NO   | YES   | NO      | YES      |
| `checkout [commit] <paths>` | NO   | YES   | YES     | **NO**   |

### 7.8 Advance Merging

```shell
# Use `git merge --abort` to abort a merge
git merge --abort

# Use `-Xignore-all-space` or `-Xignore-space-change`. The first option ignores whitespace completely when comparing lines, the second treats sequences of one or more whitespace characters as equivalent
git merge -Xignore-space-change [whitespace branch]
```

