---
title: "Rebasing"
teaching: 25
exercises: 0
questions:
- "What is rebasing?"
objectives:
- "Understand what is meant by rebasing"
- "Understand the difference between merging and rebasing"
- "When (and when not) to rebase"
keypoints:
- "rebase applies your changes on top of a new base (parent) commit"
- "rebasing rewrites history"
---

### What is it?
Rebasing is the process of moving a whole branch to a new base commit. 
Git takes your changes, and "replays" them onto the new base commit.
This creates a brand new commit for each commit in the original branch. 
As such, your history is rewritten when you rebase.

![Visual illustration of rebasing - image taken from [https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)](../fig/git-rebase.svg)

### Merge vs rebase
Imagine you create a new feature branch to work in, and meanwhile there have been
commits added to the master branch, as shown below.

![](../fig/forked-history.svg)

You have two choices for incorporating those changes into your feature branch:
merging or rebasing. We have already encountered merging, and it looks like this.

![](../fig/merge-without-rebase.svg)

The main reason you might want to rebase is to maintain a linear project history. 
In the example above, if you merge directly (recall that there are new commits on 
both the master branch and feature branch), you have a 3-way merge 
(common ancestor, HEAD and MERGE_HEAD) and a merge commit results. 
Note that you get a merge commit whether or not there are any merge conflicts.

If you rebase, the subsequent changes on `master` are replayed on top of `feature`,
creating brand new commits in the process.
If there are any merge conflicts, you are prompted to resolve these. 
Changes are thus incorporated without an extra merge commit at the end,
and you have a nice clean linear history.

![](../fig/rebase-master.svg)

### A worked example using `git rebase <base>` 

Let's start first with checking out our master branch:

```
$ git checkout master
```
{: .bash}
	
Now create and checkout a new branch.

```
$ git checkout -b results
```
{: .bash}
	
Add a results section to the paper with a couple of lines to outline the findings.
Commit the changes you made.

```
$ gedit journal.txt   # Add results section
$ git add journal.txt
$ git commit          # "Write results section"
```
{: .bash}

Checkout the **master** branch and write some conclusions. Commit them.

```
$ git checkout master # Checkout master branch
$ gedit journal.txt   # Add conclusions section
$ git add journal.txt
$ git commit          # "Write conclusions section"
```
{: .bash}
Now check out our **results** branch again and run:

```
$ git checkout results   # Check out the results branch again
$ git rebase master      # Rebase onto master
```
{: .bash}

Depending what changes we have made, there may be conflicts we have to fix in order to rebase.
If this is the case, Git will let us know, and give some instructions on how to proceed.
The process for fixing conflicts is the same as before:

```
$ gedit journal.txt      # Manually fix conficts in affected file(s)
$ git add                # Mark file as resolved
$ git rebase --continue  # Continue to rebase
```
{: .bash}

Finally, run 

```
git log -3
```
{: .bash}

and you will see that rebasing created a new commit and put it on
top of the commit pointed at by **master**.
If we switch back to the master branch, we can now merge the rebased **results** branch into
master and a linear history results.

```
$ git checkout master   # Switch branch to master
$ git merge results     # Merge results branch into master
```
{: .bash}


This [online tutorial](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)
gives a good illustration of what happens during rebasing.

> ## Warning: the perils of rebasing 
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
{: .callout}

## Interactive rebasing
`git rebase -i` will open an interactive rebasing session. This provides an opportunity
to edit, delete, combine, and reorder individual commits as they are moved onto the new
base commit. This can be useful for cleaning up history before sharing it with others.
