---
layout: page
title: Version control with Git  
subtitle: Collaborating with a remote repository
---

> ## Learning objectives {.objectives}
> * Understand how to pull changes from remote repository
> * Understand how to resolve merge conflicts

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

```{.bash}
$ cd ..  
$ ls papers
```

And let us clone our repository again, but this time specify the local
directory name,

```{.bash}
$ git clone https://github.com/<USERNAME>/papers.git papers2 
Cloning into 'papers2'...
```


So we now have two clones of our repository,

```{.bash}
$ ls 
$ papers papers2
```

Let's pretend these clones are on two separate machines! So we have 3 versions
of our repository - our two local versions, on our separate machines (we're
still pretending!) and one on GitHub. So let's go into one of our clones, add an
acknowledgements section, commit the file and push these changes to GitHub:

```{.bash}    
$ cd papers 
$ gedit journal.txt 
$ git add journal.txt 
$ git commit -m "Add acknowledgements" 
$ git push
```

Now let's change to our other repository and *fetch* the changes from our
remote repository,

```{.bash}    
$ cd ../papers2 
$ git fetch
```

We can now see what the differences are by doing,

```{.bash}    
$ git diff origin/master
```

which compares our current, `master` branch, with an `origin/master` branch
which is the name of the `master` branch in `origin` which is the alias for our
cloned repository, the one on GitHub.

We can then *merge* these changes into our current repository, which merges the
branches together,

```{.bash}    
$ git merge origin/master
```

And then we can check that we have our changes,

```{.bash}    
$ cat journal.txt 
$ git log
```

As a short-hand, we can do a Git *pull* which does a *fetch* then a *merge*. 
Now try the same process, starting in the papers2 folder, and write an abstract:

```{.bash}    
$ gedit journal.txt 
$ git add journal.txt 
$ git commit -m "Add abstract" journal.txt 
$ git push origin master
$ cd ../papers 
$ git pull origin master
```

And then check that we have our changes,

```{.bash}    
$ cat journal.txt 
$ git log
```

### Conflicts and how to resolve them

Let's continue to pretend that our two local, cloned, repositories are hosted
on two different machines, and make some changes to our file, and push these to
GitHub:

```{.bash}    
$ gedit journal.txt 
$ git add journal.txt 
$ git commit -m "Add credits - adds our names" journal.txt 
$ git push origin master
```

Now let us suppose, at a later, date, we use our other repository and we want
to change the credits.

```{.bash}    
$ cd ../papers2 
$ gedit journal.txt 
$ git add journal.txt 
$ git commit -m "Change the first author" journal.txt 
$ git push origin master
```
```{.output}
To https://github.com/<USERNAME>/papers.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/<USERNAME>/papers.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Our push fails, as we've not yet pulled down our changes from our remote
repository. Before pushing we should always pull, so let's do that...

```{.bash}    
$ git pull origin master
```

and we get:

```{.output}    
Auto-merging journal.txt
CONFLICT (content): Merge conflict in journal.txt
Automatic merge failed; fix conflicts and then commit the result.
```

As we saw earlier, with the fetch and merge, a `pull` pulls down changes from the
repository and tries to merge them. It does this on a file-by-file basis,
merging files line by line. We get a *conflict* if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before when we worked with branches and tried to merge 2 of them. If
we look at the status,

```{.bash}
$ git status
```

we can see that our file is listed as `Unmerged` and if we look at
`journal.txt`, we may see something like,

```{.output}
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

We edit the file. Then commit our changes. Now, if we push,

```{.bash}
$ git push origin master
```

All goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.


    
> # Exercises following good practice {.challenge}
> 
> #### Exercise 1: Collaborating on a remote repository
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
>
> ----
>
> #### Exercise 2: Creating branches and sharing them in the remote repository
> 
> Working with the same remote repository, each of you should create a new branch
> locally and push it back to the remote repo.
> 
> ```{.bash}
> $ git push origin new_branch
> ```
> 
> The other person should try to get the branches created by others locally
> (so eventually everybody should have the same number of branches as the remote
> repository).
> 
> To pull new branches from remote repository (into new local branches):
> 
> ```{.bash}
> $ git fetch origin 
> ```
> ```{.output}
> Counting objects: 3, done.  remote:
> Compressing objects: 100% (3/3), done.  remote: Total 3 (delta 0),
> reused 2 (delta 0) Unpacking objects: 100% (3/3), done.  From
> https://github.com/apawlik/papers 9e1705a..640210a  master     ->
> origin/master * [new branch]      new_branch -> origin/new_branch
> ```
>	
> This command gets us all branches from the remote repository (aliased as
> 'origin') but doesn't actually pull any contents.
> 
> Now we can set up a new branch locally and pull the contents of the
> corresponding remote branch into the new one which we set up:
> 
> ```{.bash}
> $ git checkout -b new_branch origin/new_branch 
> ```
> ```{.output}
> Branch new_branch set up to track remote branch new_branch from origin.
> Switched to a new branch 'new_branch'
> ```
> 	
> ----
>
> #### Exercise 3: Undoing changes using revert
> 
> Once you have the branches which others created, try to undo one of the commits.
> 
> Each one of you should try to revert a commit in a different branch to your partner(s).
> 
> Push the branch back to the remote repository. The others should pull that
> branch to get the changes you made.
> 
> What is the end result? What happens when you pull the branch that your
> colleagues changed using `revert`?	
> 	
> A: It works fine. The revert shows up in everyone's copy.
>

> # Exercises following bad practice {.challenge}
>
> #### Exercise 4: Undoing changes using reset
> 
> Now let's try to  undo a commit using `reset`.
> Remember that it is a **bad** practice to use `reset` for branches which are
> shared. The purpose of this exercise is to demonstrate what happens. 
> 
> Select a different branch for this exercise than the one you used for the
> previous one.
> 
> Reset to one of the previous commits and push it back to the remote repository.
> Pull the branches which your colleagues reset in a similar way.
> 
> What happens? Look through the history using `git log` - what can you notice?
> 
> A: You can't push the change to remote repository unless you use `push -f`.
> After force pushing the changes, your git log shows what you've done, as does
> the remote git log, but the other person's local git log shows no change to the 
> history. To that person it looks like work has just gone missing from the files
> in their working directory. Git also thinks that their branch is ahead of
> origin/branch.
>
> ----
>
> #### Exercise 5: Rebasing
> 
> This exercise is again designed to explore what happens when you follow *bad
> practice*. In this case, the bad practice is rebasing branches which are shared
> with others.
> 
> Preparation:
>
> 1. Firstly, make sure that you have all branches (so all of you working on the
> repository should have the same set of branches). 
> 2. Secondly, make sure that you all have these branches up to date. So if 
> you have any uncommited changes, commit them and push them. 
> 3. Pull the changes that your colleagues committed.
> 
> The exercise:
>
> - Pick a (non-master) branch - this time you should both be working on the same one
> - Both do some work on it and commit your changes
> - Person A should also make some changes to the `master`  branch and push them back
> - Person A should then rebase their selected non-master branch on master and push 
> everything back to the repository
> - Person B (and C if you're working in a group of 3) should now pull (previously commiting the changes they made in the  branch)
> 
> What is the end result? What happened when the other collaborator rebased the
> branch against master?
>
> A: It is confusing. When pushing person A's rebased branch back, it failed because tip
> of current branch is behing its remote counterpart. So I did a pull. Auto-merge failed
> so after editing the file to resolve the merge, add, commit, push, I finally got the 
> rebased branch pushed but with an extra commit merging changes from remote. These 
> changes were actually already in the rebased version.
> Person B then gets a merge conflict when they pull. After resolving, their history 
> is different to person A's history. One commit has a new hash ID, and others are missing.

Previous: [Working from multiple locations](08-remote.html) Next: [Pull Requests](10-pull-requests.html)
