---
title: "Branching"
teaching: 25
exercises: 15
questions:
- "What is a branch?"
- "How can I merge changes from another branch?"
objectives:
- "Know what branches are and why you would use them"
- "Understand how to merge branches"
- "Understand how to resolve conflicts during a merge"
keypoints:
- "`git branch` creates a new branch"
- "Use feature branches for new ideas and fixes, before merging into `master`"
- merging does not delete any branches
---

### What is a branch?

You might have noticed the term *branch* in status messages:

~~~
$ git status 
~~~
{: .bash}
~~~
On branch master 
nothing to commit (working directory clean)
~~~
{: .output}

and when we wanted to get back to our most recent version of the repository, we
used `git checkout master`.

Not only can our repository store the changes made to files and directories, it
can store multiple sets of these, which we can use and edit and update in
parallel. Each of these sets, or parallel instances, is termed a `branch` and
`master` is Git's default branch. 

A new branch can be created from any commit. Branches can also be *merged*
together. 

### Why are branches useful?
Suppose we've developed some software and now we want to
try out some new ideas but we're not sure yet whether we'll keep them. We
can then create a branch 'feature1' and keep our `master` branch clean. When
we're done developing the feature and we are sure that we want to include it
in our program, we can merge the feature branch with the `master` branch. 
This keeps all the work-in-progress separate from the `master` branch, which 
contains tested, working code.

When we merge our feature branch with master git creates a new commit which
contains merged files from master and feature1. After the merge we can continue
developing. **The merged branch is not deleted.** We can continue developing (and
making commits) in feature1 as well.

### Branching workflows

One popular model is the [Gitflow model](http://nvie.com/posts/a-successful-git-branching-model/):

- A `master` branch, representing a released version of the code
- A release branch, representing the beginnings of the next release - a branch 
where the code is still undergoing testing
- Various feature and/or developer-specific branches representing
work-in-progress, new features, bug fixes etc

For example:

![Feature branches ([image source)](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)](../fig/feature-branch.svg)

There are different possible workflows when using Git for code development. 
If you want to learn more about different workflows with Git, have a look at
[this discussion](https://www.atlassian.com/git/tutorials/comparing-workflows)
on the Atlassian website.


### Branching in practice

One of our colleagues wants to contribute to the paper but is not quite sure
if it will actually make a publication. So it will be safer to create a branch
and carry on working on this "experimental" version of the paper in a branch
rather than in the master.

~~~
$ git checkout -b paperWJohn
~~~
{: .bash}
~~~
Switched to a new branch 'paperWJohn'
~~~
{: .output}

We're going to change the title of the paper and update the author list (adding John Smith).
However, before we get started it's a good practice to check that we're working
on the right branch.

~~~
$ git branch			# Double check which branch we are working on
~~~
{: .bash}
~~~
  master
* paperWJohn 
~~~
{: .output}

The * indicates which branch we're currently in. Now let's make the changes to the paper.

~~~
$ gedit journal.md		# Change title and add co-author
$ git add journal.md
$ git commit			# "Modify title and add John as co-author"
~~~
{: .bash}

If we now want to work in our `master` branch. We can switch back by using:

~~~
$ git checkout master 
~~~
{: .bash}
~~~
Switched to branch 'master'
~~~
{: .output}

Having written some of the paper, we have thought of a better title for
the `master` version of the paper.

~~~
$ gedit journal			# Rewrite the title
$ git add journal.md
$ git commit			# "Rewrite title emphasising measurements"
~~~
{: .bash}

### Merging and resolving conflicts

We are now working on two papers: the main one in our `master` branch and the one
which may possibly be collaborative work in our "paperWJohn" branch.
Let's add another section to the paper to write about John's simulations.

~~~
$ git checkout paperWJohn	# Switch branch
$ gedit journal.md		# Add 'simulations' section
$ git add journal.md 
$ git commit -m "Add simulations" journal.md
~~~
{: .bash}

At this point let's just quickly visualise the state of our repo,
and we can see the forked commit history reflecting the recent work
on our two branches:

```
git log --graph --all --oneline --decorate
```
{: .bash}

```
* 89d5c6e (paperwjohn) Add simulations section
* 05d393a Modify title and add coauthor
| * (HEAD, master) bdebbe0 Rewrite title emphasising location
|/  
* 87a65e6 Add Bloggs et al paper
* 6a48241 Reference second paper in introduction
* ed26351 Reference Allen et al in introduction
* 7446b1d Write introduction
* 4f572d5 Add title and authors
```
{: .output}

After some discussions with John we decided that we will publish together,
hence it makes sense to now merge all that was authored together with John 
in branch "paperWJohn". 
We can do that by *merging* that branch with the `master` branch. Let's try
doing that:

~~~
$ git checkout master		# Switch branch
$ git merge paperWJohn		# Merge paperWJohn into master
~~~
{: .bash}
~~~
Auto-merging journal.md
CONFLICT (content): Merge conflict in journal.md
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

Git cannot complete the merge because there is a conflict - if you recall,
after creating the new branch, we changed the title of the paper on both branches.
We have to resolve the conflict and then complete the merge. We can get
some more detail

~~~
$ git status
~~~
{: .bash}
~~~
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    	both modified:      journal.md
~~~
{: .output}

Let's look inside journal.md:

```
title
<<<<<<< HEAD
Laboratory measurements of atmospheric particle formation
=======
Simulations of atmospheric particle formation
>>>>>>> paperwjohn
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to resolve the
conflict. This means removing the mark-up and doing one of:

- Keep the local version, which is the one marked-up by HEAD i.e.
"Laboratory measurements of atmospheric particle formation"

- Keep the remote version, which is the one marked-up by paperWJohn
i.e. "Simulations of atmospheric particle formation"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "Measurement-model comparison of atmospheric particle formation in laboratory experiments"

We edit the file. Then commit our changes:

~~~
$ gedit journal.md		# Resolve conflict by editing journal.md
$ git add journal.md		# Let Git know we have resolved the conflict
$ git commit -m "Rewrite title to incorporate simulations"
~~~
{: .bash}

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We can see the two branches merged if we take another look at the log graph:

```
$ git log --graph --decorate --all --oneline
```
{: .bash}

```
*   39cc80d (HEAD, master) Merge branch 'paperwjohn'
|\  
| * 89d5c6e (paperwjohn) Add simulations section
| * 05d393a Modify title and add coauthor
* | bdebbe0 Rewrite title emphasising location
|/  
* 87a65e6 Add Bloggs et al paper
* 6a48241 Reference second paper in introduction
* ed26351 Reference Allen et al in introduction
* 7446b1d Write introduction
```
{: .output}

### Looking at our history - revisited
We already looked at "going back in time with Git". But now we'll look at it in
more detail to see how moving back relates to branches and we will learn how to
actually undo things. So far we were moving back in time in one branch by 
checking out one of the past commits. 

But we were then in the "detached HEAD" state.

> ## Add a commit to detached HEAD
>
> - Checkout one of the previous commits from our repository.
> - Make some changes and commit them. What happened?
> - Now try to run `git branch`. What can you see?
>
> > ## Solution
> > ```
> > git checkout HEAD~1  # Check out the commit one before last
> > gedit journal.md     # Make some edits
> > git add journal.md   # Stage the changes
> > git commit           # Commit the changes
> > git branch           # You should see a message like the one below,
> >                      # indicating your commit does not belong to a branch
> > ```
> > {: .bash}
> > ```
> > * (detached from 57289fb)
> >   master
> > ```
> > {: .output}
> > You have just made a commit on a detached HEAD --
> > as you can see from the output above, a new temporary branch has been created,
> > which doesn't have a name.
> {: .solution} 
{: .challenge}

> ## Abandon the commit on a detached HEAD
> You decide that you want to abandon that commit.
> How would you get back to the current version of your project?
>
> > ## Solution 
> > ```
> > git checkout master
> > ```
> > {: .bash}
> > Git will warn you that you are leaving behind changes that would be lost:
> >
> > The output you see will be slightly different to that below,
> > reflecting your previous commit message and commit ID.
> >
> > ```
> > Warning: you are leaving 1 commit behind, not connected to
> > any of your branches:
> >
> > eb7c650 Add empty line for branching exercise
> >
> > If you want to keep them by creating a new branch, this may be a good time
> > to do so with:
> >
> >  git branch new_branch_name eb7c650
> >
> >  Switched to branch 'master'
> >  Your branch is up-to-date with 'master'.
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## Save your changes in a new branch
> Preparation:
>
> - You should be on the `master` branch after that last exercise.
> If not, check out master again: `git checkout master` 
> - Checkout one of the previous commits from your repository.
> - Make some changes, save the file(s), and make a commit on the detached HEAD as
> you did in the first exercise.
> - Run `git branch` to list your local branches, and see that you are on a temporary branch.
>
> This time we want to keep the commit rather than abandon it. 
> - Create a new branch and check it out.
> - Now run `git log` and see that your new commit belongs to this new branch.
> - List your local branches again and see that the temporary branch has gone.
> - Switch back to (i.e. checkout) the `master` branch 
>
> > ## Solution 
> >
> > ```
> > git checkout HEAD~1		# Checkout the commit before last
> > gedit journal.md		# Modify one of your files
> > git commit -a			# Commit all the modified files
> > git branch			# List local branches
> > ```
> > {: .bash}
> >
> > ```
> > * (HEAD detached from f908519)
> >  master
> >  paperwjohn
> > ```
> > {: .output}
> > You are currently on a temporary, unnamed branch, as indicated by the `*`.
> > ```
> > git branch dh-exercise		# Create a new branch
> > git checkout dh-exercise	# Switch to the new branch 
> > ```
> > {: .bash}
> > ```
> > Switched to a new branch 'dh-exericise'
> > ```
> > {: .output}
> > ```
> > git branch			# View local branches
> > ```
> > {: .bash}
> > ```
> > * dh-exericise
> >  master
> >  paperwjohn
> > ```
> > {: .output}
> > The commit you made on the detached HEAD now belongs to a named branch
> > (`dh-exercise` in the example above), rather than a temporary branch.
> > ```
> > git checkout master		# Switch back to the 'master' branch
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}
