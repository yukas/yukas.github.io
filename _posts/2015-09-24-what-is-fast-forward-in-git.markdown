---
layout: post
title:  "What is fast-forward in git"
date:   2015-09-24 17:47:38
categories: git
---

# What is fast-forward in git

## Problem
Sometimes you see message from git "Not possible to fast-forward, aborting". This usually happens right after you tried to pull a feature branch or do a merge.

## Fast-forward (easy merge)
Assuming we have feature branch and there were no changes made to master branch, if we were to merge it back to master, git will change master to point to tip of feature branch, and that's it, no hard work. There is practically nothing to merge, we just added new stuff.

## No fast-forward (regular merge)
In contrast, if master gone faraway after feature branch was created, git has some work to do to merge feature branch to changes introduced in master. In this exact case git throws not possible to fast-forward error, since it can't fast forward changes and fast-forward merges are default in git.

## Solution
If you encounter fast-forward error, while pulling changes or doing  merge just add --no-ff option to the command. This will make git to do regular merge, which will result in creation of so called "Merge commit". Merge commit is empty commit which sole responsibility is to save the fact what merge being done.

If you don't want this commit you can pull with `--rebase` option or do `git rebase` before the merge.

Further reading:
http://arjanvandergaag.nl/blog/clarify-git-history-with-merge-commits.html

###Exercise:
Create empty repository
Commit some change
Create feature branch
Commit some change in feature branch
Checkout to back master
Try to merge feature branch

Result: Fast-forward

Continuing...

Reset repo to before merge state
Commit another change in master branch
Try to merge feature branch again

Result: Not possible to fast-forward, aborting.

Good day!