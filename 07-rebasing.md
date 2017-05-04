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
(common ancestor, HEAD and MERGE_HEAD) and a merge commit results. If you rebase first,
a fast-forward merge results and you have a nice clean linear history.


![Visual illustration of rebasing - image taken from [https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)](fig/git-rebase.svg)

### A worked example using `git rebase <base>` 

Let's start first with checking out our master branch:

```{.bash}
$ git checkout master
```
	
Now create and checkout a new branch.

```{.bash}
$ git checkout -b results
```
	
Add a results section to the paper with a couple of lines to outline the findings.
Commit the changes you made.

```{.bash}
$ gedit journal.txt   # Add results section
$ git add journal.txt
$ git commit          # "Write results section"
```

Checkout the **master** branch and write some conclusions. Commit them.

```{.bash}
$ git checkout master # Checkout master branch
$ gedit journal.txt   # Add conclusions section
$ git add journal.txt
$ git commit          # "Write conclusions section"
```
Now check out our **results** branch again and run:

```{.bash}
$ git checkout results   # Check out the results branch again
$ git rebase master      # Rebase onto master
```

Depending what changes we have made, there may be conflicts we have to fix in order to rebase.
If this is the case, Git will let us know, and give some instructions on how to proceed.
The process for fixing conflicts is the same as before:

```{.bash}
$ gedit journal.txt      # Manually fix conficts in affected file(s)
$ git add                # Mark file as resolved
$ git rebase --continue  # Continue to rebase
```

Finally, run 

```{.bash}
git log -3
```

and you will see that rebasing created a new commit and put it on
top of the commit pointed at by **master**.
If we switch back to the master branch, we can now merge the rebased **results** branch into
master and a linear history results.

```{.bash}
$ git checkout master   # Switch branch to master
$ git merge results     # Merge results branch into master
```

![Merging with and without rebasing - image adapted from [https://www.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/dam/jcr:df39b1f1-2686-4ee5-90bf-9836783342ce/10.svg)](fig/merge-vs-rebase.svg)

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

## Interactive rebasing
`git rebase -i` will open an interactive rebasing session. This provides an opportunity
to edit, delete, combine, and reorder individual commits as they are moved onto the new
base commit. This can be useful for cleaning up history before sharing it with others.

Previous: [Undoing changes](06-undoing.html) Next: [Working with a remote
repository](08-remote.html)
