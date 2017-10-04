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
{: .bash}

```
$ papers
```
{: .output}

And let us clone our repository again, but this time specify the local
directory name,

```
$ git clone https://github.com/<USERNAME>/papers.git laptop_papers 
Cloning into 'laptop_papers'...
```
{: .bash}


So we now have two clones of our repository,

```
$ ls
```
{: .bash}

```
$ papers laptop_papers
```
{: .output}

Let's pretend these clones are on two separate machines! So we have 3 versions
of our repository - our two local versions, on our separate machines (we're
still pretending!) and one on GitHub. So let's go into one of our clones, add an
acknowledgements section, commit the file and push these changes to GitHub:

```    
$ cd papers 
$ gedit journal.txt		# Write acknowledgements section
$ git add journal.txt 
$ git commit -m "Add acknowledgements" 
$ git push
```
{: .bash}

Now let's change to our other repository and `fetch` the changes from our
remote repository,

```    
$ cd ../laptop_papers		# Switch to the other directory
$ git fetch
```
{: .bash}

We can now see what the differences are by doing,

```    
$ git diff origin/master
```
{: .bash}

which compares our current, *master* branch, with an *origin/master* branch
which is the name of the *master* branch in *origin* which is the alias for our
cloned repository, the one on GitHub.

We can then *merge* these changes into our current repository, which merges the
branches together,

```    
$ git merge origin/master
```
{: .bash}

And then we can check that we have our changes,

```    
$ cat journal.txt 
$ git log
```
{: .bash}

As a short-hand, we can do a `git pull` which does a `git fetch` then a `git merge`. 
Now try the same process, but this time starting in the laptop_papers folder (you
should already be in the laptop_papers folder), and write an abstract:

```    
$ gedit journal.txt		# Write abstract
$ git add journal.txt 
$ git commit -m "Write abstract" journal.txt 
$ git push origin master
$ cd ../papers			# Switch back to the papers directory
$ git pull origin master	# Get changes from remote repository
```
{: .bash}

And then check that we have our changes,

```    
$ cat journal.txt 
$ git log
```
{: .bash}

### Conflicts and how to resolve them

Let's continue to pretend that our two local, cloned, repositories are hosted
on two different machines. You should still be in the original *papers* folder.
Add an affilication for each author.
Then push these changes to our remote repository:

```    
$ gedit journal.txt		# Add author affiliations 
$ git add journal.txt 
$ git commit -m "Add author affiliations"
$ git push origin master
```
{: .bash}

Now let us suppose, at a later, date, we use our other repository and we want
to change the order of the authors.

```    
$ cd ../laptop_papers		# Switch directory to other copy of our repository 
$ gedit journal.txt		# Change order of the authors
$ git add journal.txt 
$ git commit -m "Change the first author" journal.txt 
$ git push origin master
```
{: .bash}
```
To https://github.com/<USERNAME>/papers.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/<USERNAME>/papers.git'
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
{: .bash}

and we get:

```    
Auto-merging journal.txt
CONFLICT (content): Merge conflict in journal.txt
Automatic merge failed; fix conflicts and then commit the result.
```
{: .output}

As we saw earlier, with the fetch and merge, a `git pull` pulls down changes from the
repository and tries to merge them. It does this on a file-by-file basis,
merging files line by line. We get a **conflict** if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before when we worked with branches and tried to merge two of them. If
we look at the status,

```
$ git status
```
{: .bash}

we can see that our file is listed as *Unmerged* and if we look at
*journal.txt*, we may see something like:

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
$ gedit journal.txt		# Edit file to resolve merge conflict
$ git add journal.txt		# Stage the file
$ git commit			# Commit to mark the conflict as resolved
$ git push origin master
```
{: .bash}
{: .bash}

... all goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

> ## Collaborating on a remote repository
> 
> In this exercise you should work with a partner or a group of three. 
> One of you should give access to your remote repository on GitHub to
> the others (by selecting Settings -> Collaborators).
> 
> Now those of you who are added as collaborators should clone the repository of
> the first person on your machines. (make sure that you **don't clone into
> a directory that is already a repository**!)
> 
> Each of you should now make some changes to the files in the repository.
> Commit the changes and then push them back to the remote repository.  
> Remember to pull changes before you push.
{: .challenge}

> ## Creating branches and sharing them in the remote repository
> 
> Working with the same remote repository, each of you should create a new branch
> locally and push it back to the remote repo.
> Each person should use a different name for their local branch.
> The following commands assume your new branch is called `new_branch` --
> you should use the name of your local branch instead of `new_branch`.
> 
> ```
> $ git push origin new_branch		# Substitute your local branch name for 'new_branch'.
> ```
> {: .bash}
> 
> The other person should try to get the branches created by others locally
> (so eventually everybody should have the same number of branches as the remote
> repository).
> 
> To pull new branches from remote repository (into new local branches):
> 
> ```
> $ git fetch origin 
> ```
> {: .bash}
> ```
> Counting objects: 3, done.  remote:
> Compressing objects: 100% (3/3), done.  remote: Total 3 (delta 0),
> reused 2 (delta 0) Unpacking objects: 100% (3/3), done.  From
> https://github.com/apawlik/papers 9e1705a..640210a  master     ->
> origin/master * [new branch]      new_branch -> origin/new_branch
> ```
> {: .output}
>	
> This command gets us all branches from the remote repository (aliased as
> 'origin') but doesn't actually pull any contents.
> 
> Now we can set up a new branch locally and pull the contents of the
> corresponding remote branch into the new one which we set up:
> 
> ```
> $ git checkout -b new_branch origin/new_branch 
> ```
> {: .bash}
> ```
> Branch new_branch set up to track remote branch new_branch from origin.
> Switched to a new branch 'new_branch'
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
