# Mercurial Notes

Docs: <https://mercurial.selenic.com/guide>

Ignore: <http://hgignore.io/>

## Global Config

The global config file is located at `~` (search for `.hgrc` on OS X or `Mercurial.ini` on Windows).

Update config: `hg config --edit`

```bash
~/.hgrc:

[ui]
username = Username <name@domain.com>
editor = nano
ignore = ~/.hgignore
```

## General

Initialize: `hg init`

Get everything ready to commit: `hg add`

Commit removed files: `hg addremove`

Commit changes: `hg commit -m "Message"` 
(Use `--amend` to update previous commit or rewrite the previous commit message.)

Only commit some files: `hg commit --include file1.txt --include file2.txt -m "Message"` (Short form: `-I`)

Commit everything but some files: `hg commit --exclude file1.txt --exclude file2.txt -m "Message"` (Short form: `-X`)

Check status: `hg status`

Check status of tracked files only: `hg status -mard`

Get current info: `hg summary`

Check current heads: `hg heads`
(Use `hg merge` to combine multiple heads)

Untrack commited files: `hg forget filename.txt`
(Takes effect with the next commit, not immediately.)

Drop uncommited changes: `hg revert -C filename.txt`
(Suppress `.orig` files with `-C` flag.)

## Branches

> A _named branch_ is a chapter. Every page has a chapter number written in the margin. This groups related pages together. ([source](http://stackoverflow.com/a/5388790/1815847))

List branches: `hg branches`

Add new branch: `hg branch branchname`

Change branch: `hg checkout branchname`

## Tags

> A _tag_ is a sticky note on a page you find interesting. It stays on the same page and can be used to find that page easily. ([source](http://stackoverflow.com/a/5388790/1815847))

List tags: `hg tags`

Add new tag: `hg tag tagname`

## Bookmarks

> A _bookmark_ is a bookmark. As you move from page to page, you move the bookmark forward with you. You can use multiple bookmarks to keep track of progress through different parts of the book. ([source](http://stackoverflow.com/a/5388790/1815847))

List bookmarks: `hg bookmarks`

Add new bookmark: `hg bookmark bookmarkname`

## Log

Show commits: `hg log`

Sow last 5 commits: `hg log --limit 5`

Show commits as graph: `hg log -G`

Pick commits from author: `hg log -u <author> -M`

## Compare

Compare modified files: `hg diff`

## Collaborate

Clone a project: `hg clone http://domain.com/repository/hello`

Pull from remote: `hg pull`

*Note:*  `hg update` must follow after `hg pull`!

Pull and update: `hg pull -u`

Force update: `hg update --check`

Pull a remote branch: `hg pull -r branchname`

Push to remote: `hg push`

Check unpushed commits: `hg out`

## Backup

Backup project: `hg archive backup.zip`

Create backup excluding .hg files: `hg archive project.tar.gz -X ".hg*"`


Restore & Rollback

Rollback previous commit: `hg rollback`

Checkout a custom revision (e.g. revision 38): `hg update -r 38`

Get rid of the changes of a special revision: `hg backout -r 38`
(Keeps history, needs new commit)

## [Strip](https://www.mercurial-scm.org/wiki/StripExtension)

Enable extension via `.hgrc`:

```bash
[extensions]
strip =
```

Delete everything til and including rev 38: `hg strip 38`
(History will be deleted, too.)

Undo `strip`: `hg unbundle /path/to/strip-backup.hg`

*Note:* Run `hg update` after restoring a backup from hg strip

## Further Notes

> Mercurial will push/pull all branches by default, while git will push/pull only the current branch.
