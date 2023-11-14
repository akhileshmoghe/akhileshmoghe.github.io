---
title:  "Git Cherry Pick - Merge only specific commits from pull request"
permalink: /_posts/devops/git/git-cherry-pick-specific-commit
date:   2018-07-22 1:33:22 +0530
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

- Cherry picking is the act of picking a commit from a branch and applying it to another.
- :warning: Cherry picking can cause duplicate commits.
- Cherry picking is a powerful and convenient command that is incredibly useful in a few scenarios. But Cherry picking should not be misused in place of `git merge` or `git rebase`.


## Usecases
- `git cherry-pick` can be useful for undoing changes. For example, say a commit is accidently made to the wrong branch. You can switch to the correct branch and cherry-pick the commit to where it should belong.
- It is usually recommended that a developer should create a separate branch for every new feature being developed, but at times features are so corelated that developer works in the same branch. And then, when he tries to create a Pull Request for only specific feature changes, the Pull Request involves all of the changes of the branch. Now, it's upto the reviewer or the person trying to merge the pull request to select and cherry pick the required commits from bunch of commits from the source branch.\
Now, as a Lead you can ask the developer to re-issue the PR with specific changes, but imagine on the day of the merge that developer is not available and you absolutely have to merge and get the build out. In such situations, `git cherry-pick` command can help you to be selective of the commits while merging it to the destination branch.
- Hot Fixes merge to main branch
  - When a bug is discovered it is important to deliver a fix to end users as quickly as possible. The developer creates an explicit commit patching this bug. This new patch commit can be cherry-picked directly to the main branch to fix the bug before it effects more users.
- Merging Stale feature commits
  - It can happen that a small low priority feature was jus got pushed to some left behind feature branch. Such couple of commits can be cherry picked and merged.


## How to cherry pick commit
- `git cherry-pick` usage is straight forward and can be executed like:
  ```shell
  $ git cherry-pick commitSha
  ```

- First go to the source branch, in which the particular commit exists.
- Use `git log` command to get the commit SHA of that commit.
- then switch the branch to the destination e.g. `main` branch, where we want to merge that commit.
  ```shell
  $ git checkout main
  ```
- Then execute the cherry-pick with the following command:
  ```shell
  $ git cherry-pick <super-long-hash-here>
  ```
- Push up this commit to remote branch like normal:
  ```shell
  $ git push origin master
  ```

---

## Options to use with git cherry-pick
### <u>Edit commit message before applying the cherry-picked commit</u>
- Passing the `-edit` option will cause git to prompt for a commit message before applying the cherry-pick operation.


### <u>Copy contents of the cherry-picked commit to working directory without commit</u>
- You may just want to merge the changes in the cherry-picked commit to current working directory changes. And avoid making the extra commit visible in `git log` or `git history`.
- The `--no-commit` option will execute the cherry pick but instead of making a new commit it will move the contents of the target commit into the working directory of the current branch.


### <u>Add signoff signature to cherry-picked commit</u>
- The `--signoff` option will add a 'signoff' signature line to the end of the cherry-pick commit message.

---

###### References:
- [Git Cherry Pick](https://www.atlassian.com/git/tutorials/cherry-pick)
- [How to merge only specific commits from a pull request with git cherry-pick](https://mattstauffer.com/blog/how-to-merge-only-specific-commits-from-a-pull-request/)
- [How to "pull request" a specific commit](https://stackoverflow.com/questions/34027850/how-to-pull-request-a-specific-commit)


