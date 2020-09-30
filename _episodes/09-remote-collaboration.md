---
title: "Collaborating with a remote repository"
teaching: 25
exercises: 15
questions:
- "How do I update my local repository with changes from the remote?"
- "How can I collaborate using Git?"
objectives:
- "Understand how to pull changes from remote repository"
- "Understand how to resolve merge conflicts"
keypoints:
- "`git pull` to integrate remote changes into local copy of repository"
---


### Pulling changes from a remote repository

Now when we have a remote repository, we can share it and collaborate with
others (and we can also work from multiple locations: for example from a laptop
and a desktop in the lab). But how do we get the latest changes? One way is
simply to clone the repository every time but this is inefficient, especially
if our repository is very large. So, Git allows us to get the latest changes
down from a repository. 

We'll first do a "dry run" of pulling changes from a remote repository and
then we'll work in pairs for some real-life practice.

First, let us leave our current local repository,

```
$ cd ..  
$ ls
```
{: .language-bash}

```
$ paper
```
{: .output}

And let us clone our repository again, but this time specify the local
directory name,

```
$ git clone https://github.com/<USERNAME>/paper.git laptop_paper 
Cloning into 'laptop_paper'...
```
{: .language-bash}


So we now have two clones of our repository,

```
$ ls
```
{: .language-bash}

```
$ paper laptop_paper
```
{: .output}

Let's pretend these clones are on two separate machines! So we have 3 versions
of our repository - our two local versions, on our separate machines (we're
still pretending!) and one on GitHub. So let's go into one of our clones, add a
figures section, commit the file and push these changes to GitHub:

```    
$ cd paper 			# Switch to the 'paper' directory
$ gedit paper.md		# Add figures section
$ git add paper.md 
$ git commit -m "Add figures"
$ git push
```
{: .language-bash}

Now let's change directory to our other repository and `fetch` the commits from our
remote repository,

```    
$ cd ../laptop_paper		# Switch to the other directory
$ git fetch
```
{: .language-bash}

We can visualise the remote branches in the same way as we did for local branches,
so let's draw a network graph before going any further:

```
git log --graph --all --decorate --oneline
```
{: .language-bash}

```
* 7c239c3 (origin/master, origin/HEAD) Add figures
* 0cc2a2d (HEAD -> master) Discuss results
* 3011ee0 Describe methodology
*   6420699 Merge branch 'simulations'
|\  
| * 7138785 (origin/simulations) Add simulations
| * e695fa8 Change title and add coauthor
* | e950911 Include aircraft in title
|/  
* 0b28b0a Explain motivation for research
* 7cacba8 Cite previous work in introduction
* 56781f4 Cite PCASP paper
* 5033467 Start the introduction
* e08262e Add title and author
```
{: .output}

As expected, we see that the `origin/master` branch is ahead of our local `master` branch
by one commit  --- note that the history hasn't diverged,
rather our local branch is missing the most recent commit on `origin/master`.

We can now see what the differences are by doing,

```    
$ git diff origin/master
```
{: .language-bash}

which compares our `master` branch with the `origin/master` branch
which is the name of the `master` branch in `origin` which is the alias for our
cloned repository, the one on GitHub.

We can then `merge` these changes into our current repository, 
but given the history hasn't diverged, we don't get a merge commit ---
instead we get a *fast-forward* merge.

```    
$ git merge origin/master
```
{: .language-bash}

```
Updating 0cc2a2d..7c239c3
Fast-forward
 paper.md | 4 ++++
 1 file changed, 4 insertions(+)
```
{: .output}

If we look at the network graph again, all that has changed
is that `master` now points to the same commit as `origin/master`.

```
git log --graph --all --decorate --oneline -4
```
{: .language-bash}

```
* 7c239c3 (HEAD -> master, origin/master, origin/HEAD) Add figures
* 0cc2a2d Discuss results
* 3011ee0 Describe methodology
*   6420699 Merge branch 'simulations'
```
{: .output}

We can inspect the file to confirm that we have our changes.

```    
$ cat paper.md 
```
{: .language-bash}

As a short-hand, we can do a `git pull` which does a `git fetch` then a `git merge`. 
Next we will update our repo using `pull`, but this time starting in the *laptop_paper* folder (you
should already be in the *laptop_paper* folder). Let's write the conclusions:

```    
$ gedit paper.md		# Write Conclusions
$ git add paper.md 
$ git commit -m "Write Conclusions" paper.md 
$ git push origin master
$ cd ../paper			# Switch back to the paper directory
$ git pull origin master	# Get changes from remote repository
```
{: .language-bash}

This is the same scenario as before, so we get another fast-forward merge.

We can check that we have our changes:

```    
$ cat paper.md 
$ git log
```
{: .language-bash}

> ## `Fetch` vs `pull`
> If `git pull` is a shortcut for `git fetch` followed by `git merge` then, why would
> you ever want to do these steps separately?
>
> Well, depending on what the commits on the remote branch contain,
> you might want to abandon your local commits before merging
> (e.g. your local commits duplicate the changes on the remote),
> rebase your local branch to avoid a merge commit, or something else.
>
> Fetching first lets you inspect the changes
> before deciding what you want to do with them.
{: .callout}

### Conflicts and how to resolve them

Let's continue to pretend that our two local, cloned, repositories are hosted
on two different machines. You should still be in the original *paper* folder.
Add an affiliation for each author.
Then push these changes to our remote repository:

```    
$ gedit paper.md		# Add author affiliations 
$ git add paper.md 
$ git commit -m "Add author affiliations"
$ git push origin master
```
{: .language-bash}

Now let us suppose, at a later date, we use our other repository (on the laptop)
and we want to change the order of the authors.

The remote branch `origin/master` is now ahead of our local `master` branch on the laptop,
because we haven't yet updated our local branch using `git pull`.

```    
$ cd ../laptop_paper		# Switch directory to other copy of our repository 
$ gedit paper.md		# Change order of the authors
$ git add paper.md 
$ git commit -m "Change the first author" paper.md 
$ git push origin master
```
{: .language-bash}
```
To https://github.com/<USERNAME>/paper.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/<USERNAME>/paper.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
{: .output}

Our push fails, as we've not yet pulled down our changes from our remote
repository. Before pushing we should always pull, so let's do that...

```    
$ git pull origin master
```
{: .language-bash}

and we get:

```    
Auto-merging paper.md
CONFLICT (content): Merge conflict in paper.md
Automatic merge failed; fix conflicts and then commit the result.
```
{: .output}

As we saw earlier, with the fetch and merge, `git pull` pulls down changes from the
repository and tries to merge them. It does this on a file-by-file basis,
merging files line by line. We get a **conflict** if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before in the *branching* episode when we merged a *feature* branch into *master*.
If we look at the status,

```
$ git status
```
{: .language-bash}

we can see that our file is listed as *Unmerged* and if we look at
*paper.md*, we see something like:

```
<<<<<<< HEAD
Author
G Capes, J Smith
=======
author
J Smith, G Capes
>>>>>>> 1b55fe7f23a6411f99bf573bfb287937ecb647fc
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to *resolve* the
conflict. Just like we did when we had to deal with the conflict when we were
merging the branches.

We edit the file. Then commit our changes. Now, if we *push* ...

```
$ gedit paper.md		# Edit file to resolve merge conflict
$ git add paper.md		# Stage the file
$ git commit			# Commit to mark the conflict as resolved
$ git push origin master
```
{: .language-bash}

... all goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We'll finish by pulling these changes into other copy of the repo,
so both copies are up to date:

```
$ cd ../paper			# Switch to 'paper' directory
$ git pull origin master	# Merge remote branch into local
```
{: .language-bash}

> ## Collaborating on a remote repository
> 
> In this exercise you should work with a partner or a group of three. 
> One of you should give access to your remote repository on GitHub to
> the others (by selecting `Settings -> Collaborators`).
> 
> Now those of you who are added as collaborators should clone the repository of
> the first person on your machines. (make sure that you **don't clone into
> a directory that is already a repository**!)
> 
> Each of you should now make some changes to the files in the repository
> e.g. fix a typo, add a file containing supplementary material.
> Commit the changes and then push them back to the remote repository.  
> Remember to pull changes before you push.
{: .challenge}

> ## Creating branches and sharing them in the remote repository
> 
> Working with the same remote repository, each of you should create a new branch
> locally and push it back to the remote repo.
>
> Each person should use a different name for their local branch.
> The following commands assume your new branch is called `my_branch`,
> and your partner's branch is called `their_branch` ---
> you should substitute the name of your new branch and your partner's new branch.
> 
> ```
> $ git checkout -b my_branch		# Create and check out a new branch.
>				 	# Substitute your local branch name for 'my_branch'.
> ```
> {: .language-bash}
>
> Now create/edit a file (e.g. fix a typo, add supplementary material etc), and then commit your changes.
>
> ```
> $ git push origin my_branch		# Push your new branch to remote repo.
> ```
> {: .language-bash}
> 
> The other person should check out local copies of the branches created by others
> (so eventually everybody should have the same number of branches as the remote
> repository).
> 
> To fetch new branches from the remote repository (into your local `.git` database):
> 
> ```
> $ git fetch origin 
> ```
> {: .language-bash}
> ```
> Counting objects: 3, done.  remote:
> Compressing objects: 100% (3/3), done.
> remote: Total 3 (delta 0), reused 2 (delta 0) Unpacking objects: 100% (3/3), done.
> From  https://github.com/gcapes/paper 
> 9e1705a..640210a master -> origin/master 
> * [new branch] their_branch -> origin/their_branch
> ```
> {: .output}
>	
> Your local repository should now contain all the branches from the remote repository,
> but the `fetch` command doesn't actually update your local branches.
> 
> The next step is to check out a new branch locally to track the new remote branch.
> 
> ```
> $ git checkout their_branch
> ```
> {: .language-bash}
> ```
> Branch their_branch set up to track remote branch their_branch from origin.
> Switched to a new branch 'their_branch'
> ```
> {: .output}
{: .challenge}

> ## Undoing changes using revert
> 
> Once you have the branches which others created, try to undo one of the commits.
>  
> Each one of you should try to [revert]({{ page.root }}/07-undoing) a commit in a different
> branch to your partner(s).
> 
> Push the branch back to the remote repository. The others should pull that
> branch to get the changes you made.
> 
> What is the end result? What happens when you pull the branch that your
> colleagues changed using `git revert`?	
> 
> > ## Solution	
> > The revert shows up in everyone's copy.
> > You should always use `revert` to undo changes which have been shared with others.
> {: .solution}
{: .challenge}
