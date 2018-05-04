---
title: "Undoing changes"
teaching: 25
exercises: 0
questions:
- "How can I discard unstaged changes?"
- "How do I edit the last commit?"
- "How can I undo a commit?"
objectives:
- "Be able to discard unstaged changes"
- "Be able to amend the most recent commit"
- "Be able to discard all changes since a particular commit"
- "Be able to undo the changes introduced by a commit"
keypoints:
- "`git checkout <file>` discards unstaged changes"
- "`git commit --amend` allows you to edit the last commit"
- "`git revert` undoes a commit, preserving history"
- "`git reset` undoes a commit by deleting history"
---

There are a number of things which we can amend and change after they have been
commited in Git.

### Discarding local changes

Maybe we made our change just to see how something looks, or to
quickly try something out. But we may be unhappy with our changes. If we
haven't get done a `git add` we can just throw the changes away and return
our file to the most recent version we committed to the repository by using:

```
$ gedit journal.md		# Make some small edits to the file
$ git checkout journal.md	# Discard edits we just made
```
{: .bash}

and we can see that our file has reverted to being the most up-to-date one in
the repository:

```
$ git status			# See that we have a clean working directory
$ gedit journal.md		# Inspect file to verify changes have been discarded
```
{: .bash}

---

### Amending the most recent commit

If you just made a commit and realised that either you did it a bit too early
and the files are not yet ready to be commited. Or, which is not as uncommon as
you think, your commit message is not as it is supposed to be. You can fix that
using the command:

```
$ git commit --amend		# Edit previous commit message
```
{: .bash}

This opens up the default editor for Git which includes the previous commit
message - you can edit it and close the editor. This will simply fix the commit
message.

But what if we forgot to include some files in the commit?

Let's try it on our example. First, let's modify two files: our paper file and
the references file. We will add a methodology section to the paper where we
detail the instrumentation used, and add a reference for this to the references
file.

```
$ gedit journal.md		# Add methodology section, including a reference
$ gedit common/references.txt	# Add new reference to references file
$ git status			# Get a status update on file modifications
```
{: .output}
{: .bash}
	
```
$ On branch master 
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   common/references.txt
	modified:   journal.md

no changes added to commit (use "git add" and/or "git commit -a")
```
{: .output}

Let's then add and commit *journal.md* but not the references file.

```
$ git add journal.md		 # Add journal to staging area
$ git commit -m "Add methodology section"
```
{: .bash}
	
Let's have a look at our working directory now:

```
$ git status
```
{: .bash}
```
$ On branch master 
Changes not staged for commit: 
  (use "git add <file>..." to update what will be committed) 
  (use "git checkout --	<file>..." to discard changes in working directory)

	modified:   common/references.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
{: .output}

Also, run `git log -2` to see what is the latest commit message and ID.

Now, we want to fix our commit and add the references file.

```
$ git add common/references.txt	# Add reference file
$ git commit --amend		# Amend most recent commit
```
{: .bash}

This will again bring up the editor and we can amend the commit message if required.

Now when we run `git status` and then `git log` we can see that our Working
Directory is clean and that both files were added. 

```
$ git status
$ git log -3
```
{: .bash}

---

### `git revert` (undo changes associated with a commit)

`git revert` removes the changes applied in a specified commit. However, rather 
than deleting the commit from history, git works out how to undo those changes
introduced by the commit, and appends a new commit with the resulting content.

Let's try it on our example. Modify the journal, describing which other instruments were
used, and then make a commit.

```
$ gedit journal.md		# Describe other instruments
$ git add journal.md
$ git commit -m "Describe Aerosol Mass Spectrometer"
```
{: .bash}

We now realise that what we've just done in our journal file is incorrect
because we are not using the data from that instrument.
Some of the data got corrupted, and due to problems with the logging computer
we are not going to use that data.
So it makes sense to abandon the commit completely.

```	
$ git revert HEAD		# Undo changes introduced by most recent commit
```
{: .bash}

When we revert, a new commit is created. The *HEAD* pointer and the branch
pointer are in fact moved forward rather than backwards. 	
	
We can revert any previous commit. That is, we can "abandon" any of the
previous changes. However, depending on the changes we made, we may bump into
a *conflict* (which we will cover in more detail further on). 

```
error: could not revert 848361e... Describe Aerosol Mass Spectrometer
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```
{: .output}
	
Behind the scenes Git gets confused trying to merge the commit *HEAD* is pointing
to with the past commit we're reverting. 

So we have seen that `git revert` is a non-destructive way to undo a commit.
What if we don't want to keep a record of undoing commits? That would give a neater
history. `git reset` can also be used to undo commits, but it does so by deleting
history.

---

### `git reset --hard` (restore a previous state by deleting history)

`git reset` has several uses, and is most often used to unstage files from the staging
area i.e. `git reset` or `git reset <file>`.

We are going to use a variant `git reset --hard <commit>` to reset things to how 
they were at `<commit>`. This is a permanent undo which deletes all changes more recent
than `<commit>` from your history. There is clearly potential here to lose work, so use
this command with care.

Let's try that on our paper, using the same example as before. Now we have two commits
which we want to abandon: the commit outlining the unreliable instrumentation, and
the subsequent revert commit. We can achieve this by resetting to the last
commit we want to keep.

We can do that by running:

```
$ git reset --hard HEAD^^	# Move tip of branch to two commits before HEAD
```
{: .bash}
```
HEAD is now at fbdc44b Add methodology section and update references file
```
{: .output}

This moves the tip of the branch back to the specified commit. If we look in-depth, 
this command moves back two pointers: `HEAD` and the pointer to the tip of the
branch we currently are working on (master). (`HEAD^` = the commit right before HEAD;
`HEAD^^` = two commits before HEAD)

The final effect is what we need: we abandoned the commits and we are now back
to where we were before making the commit about the data we are not using.

---
Click for an [animation] of the revert and reset operations we just used.

[animation]: https://learngitbranching.js.org/?NODEMO&command=git%20commit;git%20revert%20C2;git%20reset%20HEAD~2

[This article](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) discusses more in
depth `git reset` showing the differences between the three options:

* `--soft`
* `--mixed`
* `--hard`


> ## Top tip: do not use `git reset` with remote branches 
> There is one important thing to remember about the `reset` command - it
> should only be used with branches that have not been shared yet (that is they
> haven't been pushed into a remote repository that others are using). Resetting
> is changing the history **without** leaving trace. This is always a bad practice
> when using remote repositories and can lead to a horrible mess.
> 
> Reverting records the fact of "abandoning the commit" in the history.
> When we revert in a branch that is shared with others and then push that branch
> into the remote repository, it is as if we "came clean" about what we were
> doing. Everyone who pulls the branch in which we reverted changes will see it.
> With `git reset` we "keep it secret" that we have undone some changes.
> 
> As such, if we want to abandon changes in branches that are shared
> with others, we should to use the `revert` command.
{: .callout}

![Reset vs revert](../fig/git-revert-vs-reset.svg)


See this [Atlassian online tutorial](
https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting/commit-level-operations)
for further reading about the differences between `git revert` and `git reset`.

### How to undo almost anything with Git
See [this blog post](https://github.com/blog/2019-how-to-undo-almost-anything-with-git) for more example scenarios and how to recover from them.
