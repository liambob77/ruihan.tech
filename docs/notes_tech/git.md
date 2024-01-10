# Git cheat list

list of all affected files both tracked/untracked (for automation)

```bash
git status --porcelain
```

name of the current banch and nothing else (for automation)

```bash
git rev-parse --abbrev-ref HEAD
```

all commits that your branch have that are not yet in master
  
```bash
git log master..<HERE_COMES_YOUR_BRANCH_NAME>
```

setting up a character used for comments

```bash
git config core.commentchar <HERE_COMES_YOUR_COMMENT_CHAR>
```

fixing `fatal: Could not parse object` after unsuccessful revert

```bash
git revert --quit
```

view diff with inline changes (by lines)

```bash
git diff --word-diff=plain master
```

view diff of changes in a single line file (per char)

```bash
git diff --word-diff-regex=. master
```

view quick stat of a diff

```bash
git diff --shortstat master
git diff --numstat master
git diff --dirstat master
```

undo last just made commit

```bash
git reset HEAD~
```

list last 20 hashes in reverse

```bash
git log --format="%p..%h %cd %<(17)%an %s" --date=format:"%a %m/%d %H:%M" --reverse -n 20
```

list commits between dates

```bash
git log --format="%p..%h %cd %<(17)%an %s" --date=format:"%a %m/%d %H:%M" --reverse --after=2016-11-09T00:00:00-05:00 --before=2016-11-10T00:00:00-05:00
```

try a new output for diffing

```bash
git diff --compaction-heuristic ...
         --color-words ...
```

enable more thorough comparison

```bash
git config --global diff.algorithm patience
```

restoring a file from a certain commit relative to the latest

```bash
git checkout HEAD~<NUMBER> -- <RELATIVE_PATH_TO_FILE>
```

restoring a file from a certain commit relative to the given commit

```bash
git checkout <COMMIT_HASH>~<NUMBER> -- <RELATIVE_PATH_TO_FILE>
```

restoring a file from a certain commit

```bash
git checkout <COMMIT_HASH> -- <RELATIVE_PATH_TO_FILE>
```

creating a diff file from unstaged changes for a **specific folder**

```bash
git diff -- <RELATIVE_PATH_TO_FOLDER> changes.diff
```

applying a diff file, go to the root directory of your repository, run:

```bash
git apply changes.diff
```

show differences between last commit and currrent changes:

```bash
git difftool -d
```

referring to:

- last commits `... HEAD~1 ...`
- last 3 commits `... HEAD~3 ...`

show the history of changes of a file

```bash
git log -p -- ./Scripts/Libs/select2.js
```

ignoring whitespaces

```bash
git rebase --ignore-whitespace <BRANCH_NAME>
```

pulling for fast-forward only (eliminating a chance for unintended merging)

```bash
git pull --ff-only
```

- list of all tags

```bash
git fetch
git tag -l
```

archive a branch using tags

```bash
git tag <TAG_NAME> <BRANCH_NAME>
git push origin --tags
```

   you can delete your branch now

get a tagged branch

```bash
git checkout -b <BRANCH_NAME> <TAG_NAME>
```

list of all branches that haven't been merged to master

```bash
git branch --no-merge master
```

enable more elaborate diff algorithm by default

```bash
git config --global diff.algorithm histogram
```

list of all developers

```bash
git shortlog -s -n -e
```

display graph of branches

```bash
git log --decorate --graph --all --date=relative
```

   or

```bash
git log --decorate --graph --all --oneline 
```

remembering the password

```bash
git config --global credential.helper store
git fetch
```

   the first command tells git to remember the credentials that you are going to provide for the second command

path to the global config  

```bash
C:\Users\Bykov\.gitconfig
```

example of a global config  

```bash
[user]
   email = *****
   name = Aleksey Bykov
   password = *****
[merge]
   tool = p4merge
[mergetool "p4merge"]
   cmd = p4merge.exe \"$BASE\" \"$LOCAL\" \"$REMOTE\" \"$MERGED\"
   path = \"C:/Program Files/Perforce\"
   trustExitCode = false
[push]
   default = simple
[diff]
   tool = meld
   compactionHeuristic = true
[difftool "p4merge"]
   cmd = p4merge.exe \"$LOCAL\" \"$REMOTE\"
   path = C:/Program Files/Perforce/p4merge.exe
[difftool "meld"]
   cmd = \"C:/Program Files (x86)/Meld/Meld.exe\" \"$LOCAL\" \"$REMOTE\"
   path = C:/Program Files (x86)/Meld/Meld.exe
```

viewing differences between current and other branch  

```bash
git difftool -d BRANCH_NAME
```

viewing differences between current and stash  

```bash
git difftool -d stash
```

viewing differences between several commits in a diff tool  

```bash
git difftool -d HEAD@{2}...HEAD@{0}
```

view all global settings  

```bash
git config --global -l
```

delete tag

```bash
git tag -d my-tag
git push origin :refs/tags/my-tag
```

pushing tags

```bash
git push --tags
```

checking the history of a file or a folder  

```bash
git log -- <FILE_OR_FOLDER>
```

disabling the scroller  

```bash
git --no-pager <...>
```

who pushed last which branch  

```bash
git for-each-ref --format="%(committerdate) %09 %(refname) %09 %(authorname)"
```

deleting remote branch  

```bash
git push origin :<BRANCH_NAME>
```

deleting remote branch localy  

```bash
git branch -r -D <BRANCH_NAME>
```

   or to sync with the remote

```bash
git fetch --all --prune
```

deleting local branch  

```bash
git branch -d <BRANCH_NAME>
```

list **actual** remote branchs

```bash
git ls-remote --heads origin
```

list all remote (fetched) branches

```bash
git branch -r
```

list all local branches

```bash
git branch -l
```

find to which branch a given commit belongs  

```bash
git branch --contains <COMMIT>
```

updating from a forked repository

```bash
git remote add upstream https://github.com/Microsoft/TypeScript.git
git fetch upstream
git rebase upstream/master
```

add gitignore, stop tracking some ignore files

```bash
git rm -r --cached .
git ci -m "update git ignore"
```


permanently authenticating with Git repositories

```bash
git config credential.helper store
git config credential.helper 'cache --timeout 7200'
git pull origin master
```
