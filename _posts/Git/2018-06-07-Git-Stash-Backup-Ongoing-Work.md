---
title:  "Git Stash - Backup your on-going work"
permalink: /_posts/devops/git/git-stash-backup-your-on-going-work
date:   2018-06-07 1:33:22 +0530
categories:
  - Git
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Git
author: Akhilesh Moghe
show_author_profile: true
#sidebar:
# nav: Containers
---

Git, the popular distributed version control system, that offers developers a multitude of powerful commands to efficiently manage their codebases, which is also a cornerstone of modern DevOps workflows. One such command that plays a crucial role in the daily workflow of developers and DevOps professionals is 'Git Stash'. As a developer, you're probably familiar with scenarios where you need to switch branches or save your changes temporarily without committing them. That's where Git Stash comes into play, providing a convenient solution to store your work-in-progress without cluttering your commit history. In this comprehensive guide, we'll delve into the intricacies of the Git Stash command, exploring its syntax, various options, and real-world use cases. Whether you're a seasoned Git expert or just starting your journey into version control, this article will equip you with the knowledge to leverage Git Stash effectively and streamline your development process.

- Note: This article uses `git version 2.30.1`

## Stashing on-going work
- With `git stash` command, you can save the <u>uncommitted changes</u> (both <b>staged and unstaged</b>), separately for later use, and then revert them from your working copy.

```shell
$ git status
On branch main
Changes to be committed:

    new file:   new_class.cpp

Changes not staged for commit:

    modified:   new_class.hpp

$ git stash
Saved working directory and index state WIP on main: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage

$ git status
On branch main
nothing to commit, working tree clean

```

- With this, you're now free to make changes, create new commits, switch branches, and perform any other Git operations; then come back and re-apply your stash whenever necessary.
- <u><b>A stash is local to your Git repository</b></u>; stashes are not transferred to the server when you push.

---

## Re-Applying & Deleting stashed changes
- You can reapply previously stashed changes with `git stash pop`:
- Pop operation removes the changes from your `stash list` and reapplies them to the working copy.

```shell
$ git status
On branch main
nothing to commit, working tree clean
$ git stash pop
On branch main
Changes to be committed:

    new file:   new_class.cpp

Changes not staged for commit:

    modified:   new_class.hpp

Dropped refs/stash@{0} (32b3aa1d185dfe6d57b3c3cc3b32cbf3e380cc6a)
```

---

## Re-Applying but Keeping stashed changes in stash list
- Alternative to `stash pop` operation, you can reapply the stashed changes to the working copy and also keep them in the `stash list` with `git stash apply`.
- This is useful if you want to apply the same stashed changes to multiple branches. For example, some configuration settings used for build & deployment.

```shell
$ git stash apply
On branch main
Changes to be committed:

    new file:   new_class.cpp

Changes not staged for commit:

    modified:   new_class.hpp

```
---

## Re-apply a specific stash
- Get the stash version using `git stash list`.
- And just specify this version while using `git stash apply <version>` as:
```
$ git stash apply stash@{2}
```

---

## Stashing Untracked & ignored files
- <b><u>By default Git won't stash changes made to untracked or ignored files.</b></u>
- `git stash` only stash:
  - Staged changes: that have been added with `git add` command.
  - Unstaged changes: that are made to files that are currently tracked by Git.
- `git stash` does not stash:
  - New files in your working copy that have not yet been staged with `git add` command.
  - Files that have been <u><b>ignored</b></u> in `.gitignore` files.

```shell
$ touch README.md

$ git status
On branch main
Changes to be committed:

    new file:   new_class.cpp

Changes not staged for commit:

    modified:   new_class.hpp

Untracked files:

    README.md

$ git stash
Saved working directory and index state WIP on main: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage

$ git status
On branch main
Untracked files:

    README.md
```

- Adding the `-u` or `--include-untracked` option tells `git stash` to also stash your untracked files:

```shell
$ git status
On branch main
Changes to be committed:

    new file:   new_class.cpp

Changes not staged for commit:

    modified:   new_class.hpp

Untracked files:

    README.md

$ git stash -u
Saved working directory and index state WIP on main: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage

$ git status
On branch main
nothing to commit, working tree clean
```

- Use `-a` or `--all` option to include <u><b>ignored files</b></u> as well in `git stash`.

---

## Multiple Stashes
- You can run `git stash` several times to create multiple stashes, and then use `git stash list` to view them.
- By default, stashes are identified simply as a <b>"WIP" – work in progress</b> – on top of the branch and commit that you created the stash from.

```shell
$ git stash list
stash@{0}: WIP on main: 5002d47 our new homepage
stash@{1}: WIP on main: 5002d47 our new homepage
stash@{2}: WIP on main: 5002d47 our new homepage
```
---

## Rename a Git Stash
- Before Renaming the stash, you must remove the stash entry which you want to rename using as:
```
$ git stash drop stash@{1}
Dropped stash@{1} (feb72485026d71f7363983c886d6e66d1b5f40f2)
```
- Then add it again with new message using sha of commit returned after dropping:
```
$ git stash store -m "Renamed Git Stash" feb72485026d71f7363983c886d6e66d1b5f40f2
```
- Check the renamed stash:
```
$ git stash list
stash@{0}: WIP on main: 5002d47 our new homepage
stash@{1}: Renamed Git Stash
stash@{2}: WIP on main: 5002d47 our new homepage
```

---

## Save Stashes with custom messages
- If you create multiple stashes to backup different work-in-progress items, after a while it will be difficult to remember what each stash contains.
- So, to provide a bit more context, it's good practice to annotate your stashes with a description, using `git stash save "message"`:

```shell
$ git stash save "add style to our site"
Saved working directory and index state On main: add style to our site
HEAD is now at 5002d47 our new homepage

$ git stash list
stash@{0}: On main: add style to our site
stash@{1}: WIP on main: 5002d47 our new homepage
stash@{2}: WIP on main: 5002d47 our new homepage
```

- By default, `git stash pop` will re-apply the most recently created stash: `stash@{0}`.
- You can choose which stash to re-apply by passing its identifier as the last argument, for example: `$ git stash pop stash@{2}`

---

## Viewing what's in a stash
- You can view a summary of a stash with `git stash show`:

```shell
$ git stash show
 index.html | 1 +
 style.css | 3 +++
 2 files changed, 4 insertions(+)
```

- Pass the `-p` or `--patch` option to view the full diff of a stash:

```shell
$ git stash show -p
diff --git a/style.css b/style.css
new file mode 100644
index 0000000..d92368b
--- /dev/null
+++ b/style.css
@@ -0,0 +1,3 @@
+* {
+  text-decoration: blink;
+}
diff --git a/index.html b/index.html
index 9daeafb..ebdcbd2 100644
--- a/index.html
+++ b/index.html
@@ -1 +1,2 @@
+<link rel="stylesheet" href="style.css"/>
```

---

## Stash files or changes in files selectively
- You can also choose to stash just a single file, a collection of files, or individual changes from within files.
- `git stash` with `-p` option (or `--patch`) will iterate through each changed "hunk" in your working copy and ask whether you wish to stash it:
- Commonly used commands to selectively stash files or changes:
  - `/`	- search for a hunk by regex
  - `?`	- help
  - `n`	- don't stash this hunk
  - `q`	- quit (any hunks that have already been selected will be stashed)
  - `s`	- split this hunk into smaller hunks
  - `y`	- stash this hunk
- Use `CTRL+C` to abort the stashing process.

---

## Create new branch from stash for merging
- If the changes on your branch diverge from the changes in your stash, you may run into conflicts when applying your stash.
- Instead, you can use git stash branch to create a new branch to apply your stashed changes to:
- This <u>checks out a new branch</u> based on the commit that you created your stash from, and then <u>pops your stashed changes</u> onto it.

```shell
$ git stash branch add-stylesheet stash@{1}
Switched to a new branch 'add-stylesheet'
On branch add-stylesheet
Changes to be committed:

    new file:   style.css

Changes not staged for commit:

    modified:   index.html

Dropped refs/stash@{1} (32b3aa1d185dfe6d57b3c3cc3b32cbf3e380cc6a)
```

---

## Deleting the stashed changes
- You can delete it with git stash drop:

```shell
$ git stash drop stash@{1}
Dropped stash@{1} (17e2697fd8251df6163117cb3d58c1f62a5e7cdb)
```

- Or you can delete all of your stashes with:

```shell
$ git stash clear
```

---

## Git Stash - under the hood
- Stashes are actually encoded in your repository as <u>commit objects</u>.
- The special `ref` at `.git/refs/stash` points to your most recently created stash.
- Previously created stashes are referenced by the stash ref's reflog. This is why you refer to stashes by `stash@{n}:` you're actually referring to the <u>nth reflog entry for the stash ref</u>.
- Since a stash is just a commit, you can inspect it with git log:

```shell
$ git log --oneline --graph stash@{0}
*-.   953ddde WIP on main: 5002d47 our new homepage
|\ \ 
| | * 24b35a1 untracked files on main: 5002d47 our new homepage
| * 7023dd4 index on main: 5002d47 our new homepage
|/ 
* 5002d47 our new homepage
```

- for further details, please read: [How git stash works](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

---

###### References:
- [Git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)
