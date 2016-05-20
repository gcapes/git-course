---
layout: page
title: Version control with Git  
---
> ## Learning objectives {.objectives}
> * Know how to discard changes
> * Know how to amend commits
> * Understand how and when to use reset
> * Understand how and when to use revert 

There is a number of things which we can amend and change after then have been
commited in Git.

### Discarding changes

Maybe we made our change just to see how something looks, or, for code, to
quickly try something out. But we may be unhappy with our changes. If so, we
can just throw them away and return our file to the most recent version we
committed to the repository by using:

```{.bash}
$ git checkout journal.txt
```

and we can see that our file has *reverted* to being the most up-to-date one in
the repository:

```{.bash}
$ git status
```
    
### Amending commits

If you just made a commit and realised that either you did it a bit too early
and the files are not yet ready to be commited. Or, which is not as uncommon as
you think, your commit message is not as it is supposed to be. You can fix that
using the command:

```{.bash}
$ git commit --amend
```

This opens up the default editor for Git which includes the previous commit
message - you can edit it and close the editor. This will simply fix the commit
message.

But what if we forgot to include some files in the commit?

Let's try it on our example. First, let's modify two files: our paper file and
the references file. We should get something like this:
	
```{.bash}
$ git status 
```
```{.output}
$ On branch master 
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    	modified:   common/references.txt
	modified:   journal.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Let's then add and commit `journal.txt` but not the references file.

```{.bash}
$ git add journal.tex 
$ git commit -m "Adding paper but not the references"
```
	
Let's have a look what's up with our working directory now:

```{.bash}
$ git status
```
```{.output}
$ On branch master 
Changes not staged for commit: 
  (use "git add <file>..." to update what will be committed) 
  (use "git checkout --	<file>..." to discard changes in working directory)

	modified:   references/references.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Also, run `git log` to see what is the latest commit message and ID.

Now, we want to fix our commit and add the references file.

```{.bash}
$ git add common/references.txt 
$ git commit --amend
```

This will again bring up the editor and we can amend the commit message if required.

Now when we run `git status` and then `git log` we can see that our Working
Directory is clean and that both files were added. 


### Resetting

What to do if you actually want to completely abandon the commit, not just
amend it? If you're working locally, you can use the `git reset` command. 

Let's try it on our example. Modify the journal and then make a commit. We now
realise that what we've just done in our journal file makes no sense and decide
to abandon the commit completely.

We can do that by running:

```{.bash}
$ git reset --hard HEAD^
```

This moves the tip of the branch back to a previous commit. If we look in-depth, this command
moves back 2 pointers: `HEAD` and the pointer to the tip of the branch we currently are working on. (`HEAD^` = the commit right before HEAD; `HEAD^^` = two commits before HEAD)

The final effect is what we need: we abandoned the commit and we are now back
to the commit point of the previous commit.

This [article](http://git-scm.com/2011/07/11/reset.html) discusses more in
depth `git reset` showing the differences between the three options:

* `--soft` * `--mixed` * `--hard`


> ## Top tip: do not use `reset` with remote branches {.callout}
> There is however one important thing to remember about the `reset` command - it
> should only be used with branches that have not been shared yet (that is they
> haven't been pushed into a remote repository that others are using). Resetting
> is changing the history **without** leaving trace. This is always a bad practice
> when using remote repositories and can lead to a massive mess.

So what can we do if we want to abandon changes in branches that are shared
with others? We need to use the `revert` command.

### Reverting

`git revert` has a similar effect to `reset` but what happens "behind the
scenes" is very different. 

Let's try that on our example. 

```{.bash}	
$ git revert HEAD^
```

When we revert, a new commit is created. The HEAD pointer and the branch
pointer are in fact moved forward rather than backwards. 	
	
We can revert to any previous commit. That is, we can "abandon" any of the
previous changes. However, depending on the changes we made, we may bump into
a *conflict* (which we will cover in more detail further on). 

```{.output}
error: could not revert 848361e... Adds a reference to reference section
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```
	
Behind the scenes Git gets confused trying to merge the commit HEAD is pointing
to to the past commit we're reverting to. 

Reverting thus records the fact of "abandoning the commits" in the history.
When we revert in a branch that is shared with others and then push that branch
into the remote repository, it is as if we "came clean" about what we were
doing. Everyone who pulls the branch in which we reverted changes will see it.
With `reset` we "keep it secret" that we have undone some changes.
	

See this [Atlassian online tutorial](
https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting/commit-level-operations)
for further reading about the differences between `revert` and `reset`.

Previous: [Branching](03-branching.html) Next: [Rebasing](05-rebasing.html)
