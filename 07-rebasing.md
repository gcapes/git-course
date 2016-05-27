---
layout: page
title: Version control with Git  
subtitle: Rebasing
---

> ## Learning objectives {.objectives}
> * Understand what is meant by rebasing
> * Understand the difference between merging and rebasing
> * When (and when not) to rebase

### What is it?
Rebasing is the process of moving a branch to a new base commit. Git does this
by creating new commits and applying them to the specified base. As such, your
history is rewritten when you rebase. 

When working on a feature branch, you have two options for integrating your
changes into the master branch: merging directly or rebasing then merging. 

The main reason you might want to rebase is to maintain a linear project history. 
For example if you merge directly and there have been new commits on the master 
branch since you started working on a feature branch, you have a 3-way merge 
(common ancestor, HEAD and MERGE_HEAD) and a merge commit. If you rebase, a fast-
forward merge results and you have a nice clean linear history.

### A worked example using `git rebase <base>` 

Let's start first with checking out our master branch:

```{.bash}
$ git checkout master
```
	
Now create and checkout a new branch.

```{.bash}
$ git checkout -b workshopPaper
```
	
Make some changes to the files and commit them.

Checkout master branch and make some changes in it as well. Commit them.

Now check out back our "workshopPaper" branch and run:

```{.bash}
$ git rebase master
```

Run `git log` - you will see that rebasing created a new commit and put it on
top of the commit pointed at by 'master'. 


This [online tutorial](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)
gives a good illustration of what happens during rebasing.

> # Warning: the perils of rebasing {.callout}
>
> The main rule is: **do not rebase branches shared with other contributors**.
> Rebasing changes history and as with practically any Git command which changes
> history, it should be used with care. 
> 
> The branches that are pushed to remote repositories should always be merged.
> For your local branches that you never share, you may use rebasing. Rebasing is
> convenient if you want to keep a clean history. It also helps to avoid
> conflicts in the long run. But again, it is considered a better practice to use
> merge and deal with conflicts rather than mess up shared branches using rebase.


Previous: [Undoing things](04-undoing.html) Next: [Working with remote
repository](06-remote.html)
