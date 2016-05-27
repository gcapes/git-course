---
layout: page
title: Version control with Git  
subtitle: Rebasing
---

> ## Learning objectives {.objectives}
> * Understand what is meant by rebasing
> * Understand the difference between merging and rebasing
> * When (and when not) to rebase

Rebasing is the process of moving a branch to a new base commit. 

The first thing to understand about git rebase is that it solves the same problem 
as git merge. Both of these commands are designed to integrate changes from one 
branch into another branch: they just do it in very different ways.

### Rebasing vs merging

Rebasing may initially seem like merging. But in fact what happens behind the
scenes is very different. When we merge, it is clearly visible in our history
that a new commit was created from a  3-way merge. When we rebase, we change
the history but it looks as if the commits happened in a linear way. 

### Rebasing 

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

	$ git rebase master

Run `git log` - you will see that rebasing created a new commit and put it on
the top of the commit pointed by 'master'. 


This [online tutorial](http://pcottle.github.io/learnGitBranching/) shows very
well what happens during rebasing.

### Perils of rebasing

The main rule is: do not rebase branches shared with other contributors.
Rebasing changes history and as with practically any Git command that does that, it
should be used with care. 

The branches that are pushed to remote repositories should always be merged.
For your local branches that you never share, you may use rebasing. Rebasing is
convenient if you want to keep a clean history. It also helps to avoid
conflicts in the long run. But again, it is considered a better practice to use
merge and deal with conflicts rather than mess up shared branches using rebase.


Previous: [Undoing things](04-undoing.html) Next: [Working with remote
repository](06-remote.html)
