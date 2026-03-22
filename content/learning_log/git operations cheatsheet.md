---
title: 'Git Operations Cheatsheet'
date: 2026-03-22T15:46:00+01:00
draft: false
tags: ["version control", "git"]
math: true
---


## basics and setup
- `git --version`  = `git -v` - get currently installed version
- `git status`                - see status
- `git init`                  - initialize a repository in the current folder
- `git config`                - change configuration file, add `--global` for editing the global config
- `git add <file_name>`       - move the specified file to the staging area
- `git commit -m "<message>"` - commiting the files currently in the staging area, if `-m` is not used then text editor will open for message

## `.git` folder
`.git` folder is created in every repository. It contains data necessary for the `git` to operate. Some of the more interesting things:
- `HEAD`   - pointer file indicating the 'state' currently loaded (branch, commit, etc.)
- `hooks`  - folder which specifies scripts to be executed at specific moments (before commit, before push, etc.)
- `config` - INI file containing LOCAL configuration of the repository

## Areas
In git projects there are typically 4 areas:
- working dir  - the actual folder we're working on, where `git init` was called
- staging area - files after calling `git add` but before `git commit` go there
- repository   - files go there after `git commit` those are saved in log history and can be viewed
- remote       - optional cloud platform like Github or Gitlab

## `git log`
Command for displaying the commit history. Some useful flags include:
- `--oneline` - for truncated output
- `--graph`   - for visual representation of branches
- `--all`     - display commit history of all branches, not just the ones reachable from the current HEAD


## Configuration files
``` shell
/etc/gitconfig     #system wide rarerly touched
~/.gitconfig       # global standard
<repo>/.git/config # local per repository
```

config file looks like this:
``` ini
[user]
    name = Your Name
    email = your@email.com

[core]
    editor = vim

[alias]
    co = checkout
    br = branch
    st = status

[push]
    default = simple
```

you can either change the file manually or use `git config` command for example:

`git config --global user.name "<my_name>"`

Aliases are shortcuts for git commands that can be set in the config file.


## `.gitignore`
Special file for ignoring certain files from being added to the repository.

Example structure:
``` shell
# ignore normal file
example.txt
# ignore the entire folder
test_ignore/
# ignore another file
.env
```

>[!tip] multiple `.gitignore` files
You can have multiple `.gitignore` files all of which have specified relative path. This makes it easier since you don't need to go back to the file in the root directory and specify the whole path.

[https://mrkandreev.name/snippets/gitignore-generator/](.gitignore generator) - useful, you specify the technologies used and it generates commonly ignored files, useful as a starting point.

## branches
- `git branch`                 - list branches and show the currently used one
- `git branch <new_branch>`    - creates new branch with a specified name
- `git checkout <branch_name>` - switches to the specified branch if it doesn't exist it creates one
- `git checkout <commit id>`   - go to a commit specified
- `git switch`                 - similar to git checkout but doesn't create new branch by default
- `branch merge <branch_name>` - merges the specified branch INTO the currently used one
- `git rebase <branch_name>`   - combines info from a branch by putting it's commits on top of the branch specified, mostly used for keeping history clean


### Merge conflicts
When merging branches merge conflicts can arise, then a text editor will open and you have to manually fix it.

It looks like this:
``` txt
<<<< HEAD
line 1
line 2
=========
line 3
line 4
>>>>> "<branch name>"
```

You need to resolve it - keep one verion, both or parts of both and save it with:
- `git add ...`
- `git commit ...`


## `git diff`
Compares the state of a file in different versions. Shouldn't be relied on too heavily.
`---` - indicate file1
`+++` - indicate file2

Some examples:
- `--staged`                    - compares the differences between what is currently staged and not
- `<commit id 1> <commit id 2>` - compares the state of files in different commits
- `<branch 1> <branch 2>`       - compares the state of files in different branches


## `git stash`
Saves the current state without commiting.

- `git stash`                  - save the current state
- `git stash pop`              - load the last stashed state
- `git stash list`             - see stashed states
- `git stash apply <stash_id>` - apply the specific stash


>[!warning]
The stashed files are not branch specific. You can call `git stash` on one branch and `git stash pop` on another.


## Remote
- `git remote`                                    - used for connecting local repository to the cloud service
- `git remote -v`                                 - check the connection
- `git remote add <remote_name> <repository_url>` - connects the local repo to the cloud, typically `<remote_name>` is `origin`
- `git push <remote_name> <branch>`               - pushes branch onto the remote with specified name
- `git push -u <remote_name> <branch>`            - sets the remote as upstream making it "default" and allowing use of just `git push` 
- `git clone <repo_url>`                          - copies the repository with the specified url and establishes it as upstream


## Fetch and pull
Fetching is useful if data on branch has been changed and we're not sure if merging is reasonable at the moment.

- `git fetch` - downloads data from the remote repo onto the machine into `origin/<branch_name>` NOT ON `<branch_name>`. It's put into the staging area but not into the working directory

After `git fetch` we often inspect the downloaded files with commands such as:
- `git log <branch_name>..origin/<branch_name>` - shows commits reachable from `origin/<branch_name>` but not from `<branch_name>`     - meaning shows the commits that the branch on cloud repo is ahead by
- `git diff <branch_name> origin/<branch_name>` - compare local changes and the ones downloaded
- `git checkout origin/<branch_name>`           - temporarily switch to the downloaded branch without commiting what you have locally
