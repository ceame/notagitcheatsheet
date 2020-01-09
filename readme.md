All information comes from [Git Docs](https://git-scm.com/doc) 

# Git vocabulary
git term|meaning
:---|:---
`a checkout`|a working copy of the Git repository
`a tracked/untracked file`|files that were / were not present in the last snapshot. They can be (un)modified or staged 
`git typical process`|**untracked**-->`git add`-->**staged** <br> **tracked, unmodified** --> edit file --> **tracked, modified** <br> **tracked, modified** --> `git add` --> **git add** --> **staged** <br> **staged** --> `git commit` --> **commited to working copy/checkout** <br> **commited** --> `git push` --> **pushed to origin repository**
add `.gitignore` file | class of files to be added to this file if you do not want to track/add them <br> **examples** <br> `*.a` = ignore all .a files, so the names can be of zero or more characters <br> `!lib.a` = do track the `lib.a` files <br> `/TODO` = ignore the TODO file in the current working dir, but not in subdirs <br> `build/` = ignore all files in the `build` directory <br> `doc/*.txt` = ignore all `txt` files in the `doc` directory but not the other files <br> `doc/**/*.pdf` = ignore all `pdf` files in the `doc` directory and its subdirectories <br> For more best practices about `.gitignore` scripting, see [here](https://github.com/github/gitignore)
`staging` | computes SHA-1 checksum, stores file version in Git repo, adds checksum to staging area
`committing` | Git checksums each subdirectory and stores them as tree object in the Git repo,<br> Git creates a commit object that has the metadata and a pointer to the root project tree so it can re-create the snapshot
`[remote_repo]/[branch]` | if you are tracking a branch from a remote repo, it will be called like this. <br> `origin/master` is the master branch from the original remote repo, <br> while `master` is the master branch on your local repo clone

# Git Ignore set up
## Commands
symbol | meaning
---|---
blank line | separator for readability, matches no lines
a line starting with `#` | comment
`\#` | patterns starting with '#'
`example\ \Example` | a trailing space is ignored unless quoted with backslash
`!` | pattern negation, if a previous rule was excluded, this pattern negation will re-include it
`\!` | pattern including '!'
`/` | directory separator, can be at the beginning, middle or end of `.gitignore` search pattern. <br> directory separators at beginning/middle of pattern refer to directory level of the `.gitignore` path <br> a directory separator at the end of a pattern will only match diretories <br> directory separators at beginning and middle of patterns will match directories and files <br> 
`*`| matches anything except `'/'`
`?` | matches one symbol except `'/'`
`[a-zA-Z]` | range notation
`**/` | match in all directories <br> e.g. `**/foo`  matches `foo` anywhere <br> e.g. `**/foo/bar`  matches file or director `bar` anywhere directly under `foo` 
`/**` | matches anything below: <br> e.g. `abc/**`matches all files inside directory `abc` relative to the `.gitignore` file
`a/**/b` | matches zero or more directories: the example matches `a/b`, `a/x/b`, `a/x/y/b`,...
## Examples
example| meaning
---|---
`hello.*` | matches any file or folder starting with `hello`
`/hello.*` | matches any file or folder starting with `hello` in the current directory
`foo/` | directory foo and paths underneath, no file
`doc/foo` <br> `/doc/frotz` | same effect, a leading slash has no use if there is already a middle slash
`foo/*` | matches any file or folder directly under the foo directory <br> it matches `foo/test.txt` and `foo/bar` <br> it does not match `foo/bar/test.txt`


# Git commands
## Git set up
git command| what does it do
:---|:---
`git init`| initialise a new git repository locally
`git clone URL`| clone an existing repository from another source (the other source can be somewhere else on the same device or remote). <br> The URL can be `HTTPS` or `SSH` based. All files arre tracked but unmodified at this phase

## Git interacting with working copy
git command| what does it do
:---|:---
`git add [filename, folder, .]`| start tracking file, folder or all in current working directory
`git add [modified file,...]` | staging modified, tracked file
`git reset HEAD [filename]` | unstaging files
### Committing
git command| what does it do
:---|:---
`git commit -m 'your-message-here'`| commit your staged edits, including a message
`git commit --ammend`| redo commit, by including additional changes
### Removing
git command| what does it do
:---|:---
`git rm [filename]`| to remove a file entirely, so not untracked but really removed (untrack + commit)
`git rm foldername/\*.pdf`| to remove all `pdf` files entirely in the `foldername` directory, so not untracked but really removed (untrack + commit)
`git rm --cached [filename]`| to remove a file from the staging area, but stil present in the working copy
### Changing
git command| what does it do
:---|:---
`git mv [old-filename] [new-filename]` | rename a file
`git checkout -- [filename]` | unmodify a modified file
### Branching, to understand branching workflows, see [here](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
git command| what does it do
:---|:---
`git branch` | lists all current branches
`git branch -v` | lists all current branches with last commit
`git branch --merged` OR `git branch --no-merged`| lists all current branches that you have merged into your current branch OR not yet
`git branch [branchname]` | create new branch
`git checkout [branchname]` | switch to branch `branchname`
`git checkout -b [branchname]` | create and switch to branch `branchname`
`git branch -d [branchname]` | delete branch `branchname`
`git branch -vv` | shows which branches are tracked to what extent
### Merging
git command| what does it do
:---|:---
`git merge [branchname]` | you have to `git checkout [branch that you want to merge into]`. <br> Then this command to merge `branchname` into `branch that you want to merge into`. <br> After merging, you can delete the branch with `git branch -d [branchname]`
`git checkout -b [branch_name] [pulled_branch]` | Branch `branch_name` set up locally to track remote branch `pulled_branch`

### Rebasing: take all changes from one branch and replay them on a different branch
git command| what does it do
:---|:---
`git rebase [master]` | you have to `git checkout [branch that you want to apply]`. <br> Then this command to rebase `branch that you want to apply` onto `master`

## Getting information
git command| what does it do
:---|:---
`git status` OR `git status -s`| determine status of files OR brief status report
`git diff`| to see applied differences but not yet staged
`git diff --staged` or `git diff --cached`| to see applied differences that will go into the next commit
### Logging
git command| what does it do
:---|:---
`git log`| list of all commits made in that repository in reverse chronological order, all logging options are listed [here](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History#log_options)
`git log -p -2` or `git log --patch -2` | list of differences in each commit, for the 2 last entries
`git log --stat` | list of all commits made in that repository in reverse chronological order, statistics version
`git log --pretty=oneline` | list of all commits made in that repository in reverse chronological order on 1 line
`git log --pretty=short` | list of all commits made in that repository in reverse chronological order in short version
`git log --pretty=full` | list of all commits made in that repository in reverse chronological order, showing everything (just test it)
`git log --pretty=fuller` | list of all commits made in that repository in reverse chronological order, showing everything (just test it)
`git log --pretty=format:"%h - %an, %ar : %s"`| list of all commits made in that repository in reverse chronological order with customized output, [more options here](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History#pretty_format)
`git log --pretty=format:"%h : %s" --graph`| list of all commits made in that repository in reverse chronological order showing branch and merge history
`git log --since=2.weeks` OR `git log --since=2008-01-15`| list of all commits made in that repository in reverse chronological order since 2 weeks OR a specific date
`git log --author="authorname"`| list of all commits made in that repository in reverse chronological order coming from author `authorname`
`git log --grep="keyword"`| list of all commits made in that repository in reverse chronological order with the commit message containing the keyword `keyword`
`git log -S function_name`| list of all commits made in that repository in reverse chronological order that modified the occurence of the string `function_name`
`git log --pathname`| list of all commits made in that repository in reverse chronological order that introduced changes in the files in the path `pathname`, this option should always be specified at the end of the command
`git log --oneline --decorate`| lists where branch pointers are pointing
`git log --oneline --decorate --graph --all`| prints history of the commits, shows where the branch pointers are and how the history has diverged

## Communicating with remote
git command| what does it do
:---|:---
`git remote`| listing all remote handles you have specified
`git remote show [shortname]`| lists info about the remote
`git remote -v`| listing all URLs that Git stored for the shortname
`git remote add [shortname] [url]`| add new remote from `URL` giving it a name `shortname`
`git remote rename [old_shortname] [new_shortname]`| rename a remote, this also changes the remote-tracking branch names
`git fetch [shortname]`| downloads all information that the remote has but you not yet in your repository, but it does not merge it automatically with your work, you have to merge it
`git pull`| fetch + merge
`git push [where_to_push] [which_branch_to_push]`| share a branch `which_branch_to_push` with the remote repository `where_to_push`
`git push [where_to_push] [which_branch_to_push]:[new_branch_name]`| share a branch `which_branch_to_push` with the remote repository `where_to_push` and gives the branch the name `new_branch_name`
`git remote remove [shortname]` | remove a remote 

## Personalisation
git command| what does it do
:---|:---
`git config --global alias.example existing_command`| gives an alias `example` to an existing command `existing command`
`git config --global alias.unstage 'reset HEAD --'`| example of the previous
`git config --global alias.last 'log -1 HEAD'`| example of the previous

# Scenario's
## Collaboration
git flow | Situation | Other | Resource
---|---|---|---
`git pull origin master`--> `git checkout <receiving branch>` --> `git merge <merging branch>`(typically master) | When you are working on your local branch and you recently pushed your branch to the remote to be integrated in the origin/master. When you want to pull the most recent origin/master to be able to include those changes in you local branch. | none | https://www.atlassian.com/git/tutorials/using-branches/git-merge
