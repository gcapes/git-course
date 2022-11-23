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
{: .language-bash}
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

![Feature branches ([image
source)](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)](../fig/feature-branch.svg)

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
$ git checkout -b simulations
~~~
{: .language-bash}
~~~
Switched to a new branch 'simulations'
~~~
{: .output}

We're going to change the title of the paper and update the author list (adding John Smith).
However, before we get started it's a good practice to check that we're working
on the right branch.

~~~
$ git branch			# Double check which branch we are working on
~~~
{: .language-bash}
~~~
  master
* simulations
~~~
{: .output}

The * indicates which branch we're currently in. Now let's [make the changes][change-title]
to the paper.

~~~
$ nano paper.md		# Change title and add co-author
$ git add paper.md
$ git commit			# "Modify title and add John as co-author"
~~~
{: .language-bash}

If we now want to work in our `master` branch. We can switch back by using:

~~~
$ git checkout master
~~~
{: .language-bash}
~~~
Switched to branch 'master'
~~~
{: .output}

Having written some of the paper, we have thought of a [better title][aircraft-title] for
the `master` version of the paper.

~~~
$ nano paper.md		# Rewrite the title
$ git add paper.md
$ git commit			# "Include aircraft in title"
~~~
{: .language-bash}

### Merging and resolving conflicts

We are now working on two papers: the main one in our `master` branch and the one
which may possibly be collaborative work in our "simulations" branch.
Let's [add another section][simulations-section] to the paper to write about John's simulations.

~~~
$ git checkout simulations	# Switch branch
$ nano paper.md		# Add 'simulations' section
$ git add paper.md
$ git commit -m "Add simulations" paper.md
~~~
{: .language-bash}

At this point let's visualise the state of our repo,
and we can see the diverged commit history reflecting the recent work
on our two branches:

```
git log --graph --all --oneline --decorate
```
{: .language-bash}

```
* 89d5c6e (simulations) Add simulations
* 05d393a Change title and add coauthor
| * (HEAD, master) bdebbe0 Include aircraft in title
|/
* 87a65e6 Explain motivation for research
* 6a48241 Cite previous work in introduction
* ed26351 Cite PCASP paper
* 7446b1d Start the introduction
* 4f572d5 Add title and author
```
{: .output}

After some discussions with John we decided that we will publish together,
hence it makes sense to now merge all that was authored together with John
in branch "simulations".
We can do that by *merging* that branch with the `master` branch. Let's try
doing that:

~~~
$ git checkout master		# Switch branch
$ git merge simulations		# Merge simulations into master
~~~
{: .language-bash}
~~~
Auto-merging paper.md
CONFLICT (content): Merge conflict in paper.md
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
{: .language-bash}
~~~
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:	    paper.md
~~~
{: .output}

Let's look inside paper.md:

```
# Title
<<<<<<< HEAD
Aircraft measurements of biomass burning aerosols over West Africa
=======
Simulations of biomass burning aerosols over West Africa
>>>>>>> simulations
```
{: .output}

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to resolve the
conflict. This means removing the mark-up and doing one of:

- Keep the current version, which is the one marked-up by HEAD i.e.
"Aircraft measurements of biomass burning aerosols over West Africa"

- Keep the version from the other branch, which is the one marked-up by simulations
i.e. "Simulations of biomass burning aerosols over West Africa"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "Aircraft measurements and simulations of biomass burning aerosols over West
Africa"

[We edit the file][resolve-conflict]. Then commit our changes:

~~~
$ nano paper.md		# Resolve conflict by editing paper.md
$ git add paper.md		# Let Git know we have resolved the conflict
$ git commit
~~~
{: .language-bash}

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We can see the two branches merged if we take another look at the log graph:

```
$ git log --graph --decorate --all --oneline
```
{: .language-bash}

```
*   39cc80d (HEAD, master) Merge branch 'simulations'
|\
| * 89d5c6e (simulations) Add simulations
| * 05d393a Change title and add coauthor
* | bdebbe0 Include aircraft in title
|/
* 87a65e6 Explain motivation for research
* 6a48241 Cite previous work in introduction
* ed26351 Cite PCASP paper
* 7446b1d Start the introduction
* 4f572d5 Add title and author
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
> > nano paper.md     # Make some edits
> > git add paper.md   # Stage the changes
> > git commit		 # Commit the changes
> > git branch		 # You should see a message like the one below,
> >			 # indicating your commit does not belong to a branch
> > ```
> > {: .language-bash}
> > ```
> > * (detached from 57289fb)
> >   master
> > ```
> > {: .output}
> > You have just made a commit on a detached HEAD --
> > as you can see from the output above, a new temporary branch has been created,
> > which doesn't have a name.
> >
> > See this [detached HEAD animation] of the above process.
> {: .solution}
{: .challenge}

[detached HEAD animation]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit

> ## Abandon the commit on a detached HEAD
> You decide that you want to abandon that commit.
> How would you get back to the current version of your project?
>
> > ## Solution
> > ```
> > git checkout master
> > ```
> > {: .language-bash}
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
> > See this [abandon detached HEAD] animation.
> {: .solution}
{: .challenge}

[abandon detached HEAD]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit;git%20checkout%20master

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
> > nano paper.md		# Modify one of your files
> > git commit -a			# Commit all the modified files
> > git branch			# List local branches
> > ```
> > {: .language-bash}
> >
> > ```
> > * (HEAD detached from f908519)
> >  master
> >  simulations
> > ```
> > {: .output}
> > You are currently on a temporary, unnamed branch, as indicated by the `*`.
> > ```
> > git branch dh-exercise		# Create a new branch
> > git checkout dh-exercise	# Switch to the new branch
> > ```
> > {: .language-bash}
> > ```
> > Switched to a new branch 'dh-exericise'
> > ```
> > {: .output}
> > ```
> > git branch			# View local branches
> > ```
> > {: .language-bash}
> > ```
> > * dh-exericise
> >  master
> >  simulations
> > ```
> > {: .output}
> > The commit you made on the detached HEAD now belongs to a named branch
> > (`dh-exercise` in the example above), rather than a temporary branch.
> > ```
> > git checkout master		# Switch back to the 'master' branch
> > ```
> > {: .language-bash}
> > See this [new branch animation] for the key points in this exercise.
> {: .solution}
{: .challenge}

[new branch animation]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit;git%20branch%20dh-ex;git%20checkout%20dh-ex;git%20checkout%20master
[change-title]: https://github.com/gcapes/git-course-paper/commit/6d3e6fb24213f796a36fc8bec9db4cc779685482#diff-0403ef06adf405f7b310b4518bd6a3559854f54c61676f676ce9cbfee7172ab6
[aircraft-title]: https://github.com/gcapes/git-course-paper/commit/adcd2a0ceba5d87f8c56873d7984020fa3d82809
[simulations-section]: https://github.com/gcapes/git-course-paper/commit/804bd97911c7b8191e5a372df364e87d27ea8c05
[resolve-conflict]: https://github.com/gcapes/git-course-paper/commit/366bc48f3529ea740f5345b2307e105b942ad48b

{% include links.md %}
