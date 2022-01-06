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
- "`rebase` applies your changes on top of a new base (parent) commit"
- "rebasing rewrites history"
---

We were in the *paper* directory at the end of the last episode,
which is where this episode continues.

Let's review the recent history of our project,
noting particularly the commit message which results when `origin/master` and `master` diverge,
and `origin/master` is merged back into `master`.

```
$ git log --graph --all --oneline --decorate -6
```
{: .language-bash}

```
*   365748e (HEAD -> master, origin/master, origin/HEAD) Merge branch 'master' of github.com:gcapes/paper
|\
| * ff18da4 Add author affiliations
* | 8f44540 Change first author
|/
* 8494909 Write conclusions
* e90a501 Add figures
* 3011ee0 Discuss results
```
{: .output}

Normally a merge commit indicates that a feature branch has been completed,
a bug has been fixed, or marks a release version of our project.
Our most recent merge commit doesn't mark any real milestone in the history of the project ---
all it tells us is that we didn't pull before we tried to push.
Merge commits like this don't add any real value[^opinion],
and can quickly clutter the history of a project.

If only there were a way to avoid them,
e.g. by starting with the tip of the remote branch
and reapplying our local commits from this new starting point.
You could also describe this as moving the local commits onto a new *base* commit
i.e. **rebasing**.


### What is it?
Rebasing is the process of moving a whole branch to a new base commit.
Git takes your changes, and "replays" them onto the new base commit.
This creates a brand new commit for each commit in the original branch.
As such, **your history is rewritten when you rebase**.

It's like saying "add my changes to what has already been done".

![Visual illustration of rebasing - image taken from
[https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)](../fig/git-rebase.svg)

### How's that different to merging?
Imagine you create a new feature branch to work in, and meanwhile there have been
commits added to the `master` branch, as shown below.

![](../fig/forked-history.svg)

You've finished working on the feature, and
you want to incorporate your changes from the `feature` branch into the `master` branch.
You could merge directly or rebase then merge. We have already encountered merging, and it
looks like this:

![](../fig/merge-without-rebase.svg)

The main reason you might want to rebase is to maintain a linear project history.
In the example above, if you merge directly (recall that there are new commits on
both the `master` branch and `feature` branch), you have a 3-way merge
(common ancestor, HEAD and MERGE_HEAD) and a merge commit results.
Note that you get a merge commit whether or not there are any merge conflicts.

If you rebase, your commits from the `feature` branch are replayed onto `master`,
creating brand new commits in the process.
If there are any merge conflicts, you are prompted to resolve these.

![](../fig/rebase-master.svg)

After rebasing, you can then perform a fast-forward merge into `master` i.e. without
an extra merge commit at the end, so you have a nice clean linear history.

![](../fig/rebase-then-merge.svg)

### Why would I consider rebasing?
`Rebase` and `merge` solve the same problem: integrating commits from one branch into another.
Which method you use is largely personal preference.

Some reasons to consider rebasing:
- To give a linear project history, which is easier to follow
	- This makes using `git log`, and `git bisect` easier
- To integrate upstream changes into your local repository, without creating any merge commits
- To keep a feature branch up to date with master, without polluting your feature branch with
extraneous merge commits
- Makes pull requests easier to manage (because you've already resolved any merge conflicts
while rebasing)
- To tidy up a feature branch before merging into master (requires interactive rebase)


> ## Interactive rebasing
> `git rebase -i` will open an interactive rebasing session. This provides an opportunity
> to edit, delete, combine, and reorder individual commits as they are moved onto the new
> base commit. This can be useful for cleaning up history before sharing it with others.
{: .callout}

### A worked example using `git rebase <base>`

We'll repeat the scenario from the last episode where the local and remote branches diverge,
but instead of merging the remote branch `origin/master` into `master`,
we'll rebase `master` onto `origin/master`.

We'll [write some acknowledgements][acknowledgements], then commit and push.

```
$ gedit paper.md				# Write acknowledgements
$ git add paper.md
$ git commit -m "Write acknowledgements section"
$ git push origin master			# Push master branch to remote
```
{: .language-bash}


We'll now switch machine to our laptop, and [write the abstract][abstract]:

```
$ cd ../laptop_paper				# Pretend we're on the laptop
$ gedit paper.md				# Add abstract section
$ git add paper.md
$ git commit					# "Write abstract"
```
{: .language-bash}

At this point we can view a graph of project history,
and see where the `master` branch diverges from `origin/master`:

```
$ git fetch					# Retrieve information about remote branches
$ git log --graph --all --oneline --decorate	# View project history before rebasing
```
{: .language-bash}

```
* 21cfe5f (HEAD -> master) Write abstract
| * 13aa7e3 (origin/master, origin/HEAD) Add acknowledgements
|/
*   365748e Merge branch 'master' of github.com:gcapes/paper
|\
| * ff18da4 Add author affiliations
* | 8f44540 Change first author
|/
* 8494909 Add figures

```
{: .output}

As before, if we try to push our local branch, it will fail ---
git will suggest that we `pull` in order to merge the remote commit into our local branch,
before pushing again.
We did that in the last episode, which resulted in a 'forgot-to-pull' merge commit.
This time we will replay our local branch onto to the remote branch.

```
$ git rebase origin/master			# Rebase current branch onto origin/master
```
{: .language-bash}

Note that this syntax only works because we just did a `git fetch`.
Typically, you would use `git pull --rebase` instead, which combines the fetch and rebase steps.

> ## Merge conflicts during a rebase
> Depending what changes we have made, there may be conflicts we have to fix in order to rebase.
> If this is the case, Git will let us know, and give some instructions on how to proceed.
> The process for fixing conflicts is the same as before:
>
> ```
> $ gedit file					# Manually fix conficts in affected file(s)
> $ git add file					# Mark file(s) as resolved
> $ git rebase --continue				# Continue to rebase
> ```
> {: .language-bash}
{: .callout}

Let's now visualise our project history again, having rebased `master` onto `origin/master`,
and observe that we now have a linear project history.
Rebasing has created a new commit (with a new commit ID) and put it on top of
the commit pointed at by `origin/master` --- thus avoiding that forgot-to-pull merge commit!

```
$ git log --graph --all --oneline --decorate	# View project history after rebasing
```
{: .language-bash}

```
* 6105e61 (HEAD -> master) Write abstract
* 13aa7e3 (origin/master, origin/HEAD) Add acknowledgements
*   365748e Merge branch 'master' of github.com:gcapes/paper
|\
| * ff18da4 Add author affiliations
* | 8f44540 Change first author
|/
* 8494909 Add figures
```
{:  .output}

Having integrated the remote changes into our local branch, we can now push our local branch
back to 'origin'.

```
$ git push origin master
```
{: .language-bash}

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

[^opinion]: This statement contains elements of opinion.
[acknowledgements]: https://github.com/gcapes/git-course-paper/commit/db0d7c74b532979242525cff88830f743f5e5bdc
[abstract]: https://github.com/gcapes/git-course-paper/commit/3c7af5d5ec501c94729c222fe3271da0c03c7265

{% include links.md %}
