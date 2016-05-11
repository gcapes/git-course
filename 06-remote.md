---
layout: page
title: Version control with Git  
---
## Working from multiple locations with a remote repository

We're going to set up a remote repository that we can use from multiple
locations. The remote repository can also be shared with colleagues, if we want
to.

### GitHub

[GitHub](http://GitHub.com) is a company which provides remote repositories for
Git and a range of functionalities supporting their use. GitHub allows users to
set up  their private and public source code Git repositories. It provides
tools for browsing, collaborating on and documenting code. GitHub, like other
services such as [Launchpad](https://launchpad.net),
[Bitucket](https://bitbucket.org), [GoogleCode](http://code.google.com), and
[SourceForge](http://sourceforge.net) supports a wealth of resources to support
projects including:

* Time histories changes to repositories * Commit-triggered e-mails * Browsing
code from within a web browser, with syntax highlighting * Software release
management * Issue (ticket) and bug tracking * Download * Varying permissions
for various groups of users * Other service hooks e.g. to Twitter.

**Note**  GitHub's free repositories have public licences **by default**. If
you don't want to share (in the most liberal sense) your stuff with the world
and you want to use GitHub (instead of GitHub), you will need to pay for the
private GitHub repositories (GitHub offers up to 5 free private repositories,
if you are an academic - but do check this information as T&C may change).

### GitHub for research GitHub **isn't** the only remote repostitories
provider. It is however very popular, in particular within the Open Source
communities. The reason why we teach GitHub in this tutorial is mainly due to
popular demand. 

Also, GitHub has started working intensively in providing different
[functionalities which are particularily useful in research
context](https://github.com/blog/1840-improving-github-for-sciences).   

### Get an account

Let's get back to our tutorial. We will first need a GitHub account.

Setting up GitHub at first requires a user name and password. [Sign
up](https://GitHub.com) or if you already have an account [sign
in](https://GitHub.com). 

### Create a new repository

Now, we can create a repository on GitHub,

* Log in to [GitHub](https://GitHub.com/) * Click on the Create icon on the top
right * Enter Repository name: "bootcamp-WiSE" * For the purpose of this
exercise we'll create a public repository * Make sure the Initialize this
repository with a README is *unselected* * Click Create Repository

You'll get a page with new information about your repository. We already have
our local repository and we will be *pushing* it to GitHub. 

    git remote add origin https://github.com/USERNAME/papers.git git push -u
    origin master

This sets up an alias, `origin`, to correspond to the URL of our new repository
on GitHub.

Now copy and paste the second line,

    $ git push -u origin master Counting objects: 38, done.  Delta compression
    using up to 4 threads.  Compressing objects: 100% (30/30), done.  Writing
    objects: 100% (38/38), 3.59 KiB, done.  Total 38 (delta 9), reused 0 (delta
    0) To https://github.com/USERNAME/papers.git * [new branch]      master ->
    master Branch master set up to track remote branch master from origin.


This *pushes* our `master` branch to the remote repository, named via the alias
`origin` and creates a new `master` branch in the remote repository.

Now, on GitHub, we should see our code and click the Commits tab we should see
our complete history of commits.  

Our local repository is now available on GitHub. So, anywhere we can access
GitHub, we can access our repository.



### Remote repository and branching

  
Let's push all our local branches into our remote repository:

    $ git push origin branch_name
    
    
This will work assumig that 'origin' is still an alias for our remote
repository in GitHub. The branch should now be created in our GitHub
repository.    

To list all branches (local and remote):

    $ git branch -a
    

To delete the remote branch:

    $ git push origin :feature2 



### Cloning a remote repository

Now, let's do something drastic! (but before that step, make sure that you
pushed all your local branches into the remote repository)

    $ cd ..  $ rm -rf papers

Gulp! We've just wiped our local repository! But, as we've a copy on GitHub we
can just copy, or *clone* that,

    $ git clone https://github.com/USERNAME/papers.git Cloning into 'papers'…
    remote: Counting objects: 38, done.  remote: Compressing objects: 100%
    (21/21), done.  remote: Total 38 (delta 9), reused 38 (delta 9) Unpacking
    objects: 100% (38/38), done.

Cloning creates an exact copy of the repository. By deafult it creates
a directory with the same name as the name of the repository. 

Now, if we change into `papers` we can see that we have our repository,

    $ cd papers $ git log

and we can see our Git configuration files too,

    $ ls -A


### Push changes to a remote repository

We can use our cloned repository just as if it was a local repository so let's
make some changes to our files and commit these.

Having done that, how do we send our changes back to the remote repository? We
can do this by *pushing* our changes,

    $ git push

If we now check our GitHub page we should be able to see our new changes under
the Commit tab.

To see all remote repositories (we can have multiple ones!) type:
	
	$ git remote -v



### Collaboration: pulling changes from a remote repository

Now when we have a remote repository, we can share it and collaborate with
others (and we can also work from multiple locations: for example from a laptop
and a desktorp in the lab). But how do we get the latest changes? One way is
simply to clone the repository every time but this is inefficient, especially
if our repository is very large. So, Git allows us to get the latest changes
down from a repository. 

We'll first do some "dry run" of pulling changes from a remote repository and
then we'll work in pairs for some real-life practice.

First, let us leave our current local repository,

    $ cd ..  $ ls papers

And let us clone our repository again, but this time specify the local
directory name,

    $ git clone https://github.com/USERNAME/papers.git papers2 Cloning into
    'papers2'...


So we now have two clones of our repository,

    $ ls $ papers papers2

Let's pretend these clones are on two separate machines! So we have 3 versions
of our repository - our two local versions, on our separate machines (we're
still pretending!) and one on GitHub. So let's go into one of our clones, make
some changes, commit these and push these to GitHub:

    $ cd papers $ nano journal.txt $ git add journal.txt $ git commit -m "Added
    some more comments" $ git push

Now let's change to our other repository and *fetch* the changes from our
remote repository,

    $ cd ../papers2 $ git fetch

We can now see what the differences are by doing,

    $ git diff origin/master

which compares our current, `master` branch, with an `origin/master` branch
which is the name of the `master` branch in `origin` which is the alias for our
cloned repository, the one on GitHub.

We can then *merge* these changes into our current repository, which merges the
branches together,

    $ git merge origin/master

And then we can check that we have our changes,

    $ cat journal.txt $ git log

As a short-hand, we can do a Git *pull* which does a *fetch* then a *merge*,

    $ nano journal.txt $ git add journal.txt $ git commit -m "Added
    acknowldgements" journal.txt $ git push $ cd ..  $ cd ../papers $ git pull

And then check that we have our changes,

    $ cat journal.txt $ git log

### Collaboration: conflicts and how to resolve them

Let's continue to pretend that our two local, cloned, repositories are hosted
on two different machines, and make some changes to our file, and push these to
GitHub:

    $ nano journal.txt $ git add journal.txt $ git commit -m "Credits - added
    our names" journal.txt $ git push

Now let us suppose, at a later, date, we use our other repository and we want
to change the credits.

    $ cd ../papers2 $ nano journal.txt $ git add journal.txt $ git commit -m
    "Changed the first author" journal.txt $ git push

Our push fails, as we've not yet pulled down our changes from our remote
repository. Before pushing we should always pull, so let's do that...

    $ git pull

and we get:

    Auto-merging journal.txt CONFLICT (content): Merge conflict in journal.txt
    Automatic merge failed; fix conflicts and then commit the result.

As we saw earlier, with the fetch and merge, a pull pulls down changes from the
repository and tries to merge these. It does this on a file-by-file basis,
merging files line by line. We get a *conflict* when if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before when we worked with branches and tried to merge 2 of them. If
we look at the status,

    $ git status

we can see that our file is listed as `Unmerged` and if we look at
`journal.txt`, we may see something like,

    <<<<<<< HEAD Authors: Aleksandra Pawlik, John Smith ======= Authors: John
    Smith, Aleksandra Pawlik >>>>>>> 71d34decd32124ea809e50cfbb7da8e3e354ac26 

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to *resolve* the
conflict. Just like we did when we had to deal with the conflict when we were
merging the branches.

We edit the file. Then commit our changes. Now, if we push,

    $ git push

All goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.


    

## Exercises

### Exercise 1: Collaborating on remote repository

In this exercise you should work with a partner or a small group (probably not
more than 3 people). One of you should give access to his/her remote repository
on GitHub to the others (by selecting Settings -> Collaborators).

Now those of you who are added as collaborators should clone the repository of
the first person on your machines. (make sure that you **don't clone into
a diectory that is already a repository**!)

Each one of you should now make some changes to the files in the repository.
Commit the changes and then push them back to the remote repository. 

### Exercise 2: Creating branches and sharing them in the remote repository

Working with the same remote repository, create 2 branches locally and push
them back to the remote repo.

	$ git push origin new_branch

The other person(s) should try to get the branches created by others locally
(so eventually everybody should have the same number of branches as the remote
repository).

To pull new branches from remote repository (into new local branches):

	$ git fetch origin remote: Counting objects: 3, done.  remote:
	Compressing objects: 100% (3/3), done.  remote: Total 3 (delta 0),
	reused 2 (delta 0) Unpacking objects: 100% (3/3), done.  From
	https://github.com/apawlik/papers 9e1705a..640210a  master     ->
	origin/master * [new branch]      new_branch -> origin/new_branch

This command gets us all branches from the remote repository (aliased as
'origin') but doesn't actually pull any contents.

Now we can set up a new branch locally and pull the contents of the
corresponding remote branch into the new one which we set up:

	$ git checkout -b new_branch origin/new_branch Branch new_branch set up
	to track remote branch new_branch from origin.  Switched to a new
	branch 'new_branch'
	
	
### Exercise 3: Undoing changes using revert

Once you have the branches which others created try in one of the branches to
undo one of the commits. 

Ideally, each one of you should try to do it in a different branch than the
others.

Push the branch back to the remote repository. The othes should pull that
branch to get the changes you made.

What is the end result? What happens when you pull the branch that your
colleagues changes using `revert`?
	
	
	
### Exercise 4: Undoing changes using reset


Now let's try a similar thing like in the Exercise above but using `reset`.
Remember that this is a **bad** practice to use `reset` for branches which are
shared. The purpose of this exercise is to actually try to prove that. 

Select a different branch for this exercise than the one you used for the
previous one.

Reset to one of the previous commits and push it back to the remote repository.
Pull the branches which your colleagues reset in a similar way.

What happens? Look through the history using `git log` - what can you notice?

	    
### Exercise 5: Rebasing

This exercise is again designed to explore what happens when you follow *bad
practice*. In this case, the bad practice is rebasing branches which are shared
with others.

Firstly, make sure that you have all branches (so all of you working on the
repository should have the same set of branches). Secondly, make sure that your
all have these branches up to date. So if you have any uncommited changes,
commit them and push them. Pull the changes that your colleagues commited.


Pick a branch - this time pick all of you should try picking the same one. Do
some work on it.   One of you should also make some changes to the `master`
branch and push them back.  The same person should then rebase their selected
non-master branch on master and push everything back to the repository. The
others should now pull (previously commiting the changes they made in the
branch).

What is the end result? What happened when the other collaborator rebased the
branch against master?





Previous: [Rebasing](05-rebasing.html) Next: [Pull Requests](07-pull-requests.html)
