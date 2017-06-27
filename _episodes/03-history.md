---
title: "Looking at history and differences"
teaching: 30
exercises: 15 (inc 10 for break)
questions:
- "How do I get started with Git?"
- "Where does Git store information?"
objectives:
- "Be able to view history of changes to a repository"
- "Be able to view differences between commits"
- "Understand how and when to use tags to label commits"
keypoints:
- "`git log` shows the commit history"
- "`git diff` displays differences between commits"
- "`git checkout` recovers old versions of files"
- "`HEAD` points to the commit you have checked out"
- "`master` points to the tip of the master branch"
---


### Looking at differences

We forgot to reference a second paper in the introduction section.
Correct it, save the file but do not commit it yet.
We can review the changes that we made using:

~~~
$ gedit journal.txt    # Add second reference to introduction
$ git diff journal.txt # View changes to file
~~~
{: .bash}

This shows the difference between the latest copy in the repository and the
changes we made. 

* `-` means a line was deleted.  
* `+` means a line was added.  
* Note that a line that has been edited is shown as a removal of the old line and an
addition of the updated line.

Looking at differences between commits is one of the most common activities.
The `git diff` command itself has a number of [useful
options](http://git-scm.com/docs/git-diff.html).

There is also a more elaborate front end to the `git diff` command - `git
difftool`. `git difftool` is used for comparing and editing files that
changed between commits using common diff tools. There is a range of such
tools, including diffmerge, meld, and vimdiff.
Configuring a visual diff tool is covered on the
[hints and tips](11-hints-and-tips.html) page.

There is a range of GUI-based tools supporting looking at differences and
editing files. For example:

* [Diffmerge](https://sourcegear.com/diffmerge/) (Free, cross-platform)
* [WinMerge](http://winmerge.org/) - open source tool available for Windows;
* GitHub [Compare
view](https://help.github.com/articles/comparing-commits-across-time)


The choice of GUI for viewing differences depends on the context in which you
are working and your own preferences related to choosing tools and
technologies.

Now commit the change we made by adding the second reference:
```
$ git add journal.txt
$ git commit          # "Reference second paper in introduction"
```
{: .bash}

### Looking at our history

To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

~~~
$ git log
~~~
{: .bash}

The output shows: the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit, author, date, and your
comment. The `git log` command has many options to print information in various
ways, for example:

~~~
$ git log --relative-date
~~~
{: .bash}

Git automatically assigns an identifier (*COMMITID*) to each commit made to the
repository. In order to see the changes made between any earlier commit and our
current version, we can use  `git diff`  providing the commit identifier of the
earlier commit:

~~~
$ git diff COMMITID   # View differences between current version and COMMITID
~~~
{: .bash}

And, to see changes between two commits:

~~~
$ git diff OLDER_COMMITID NEWER_COMMITID
~~~
{: .bash}

Using our commit identifiers we can set our working directory to contain the
state of the repository as it was at any commit. So, let's go back to the very
first commit we made,

~~~
$ git log 
$ git checkout COMMITID
~~~
{: .bash}

We will get something like this:

~~~
Note: checking out '21cfbdec'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name
  
HEAD is now at 21cfbde... Add title and authors
~~~
{: .output}

This strange concept of the 'detached HEAD' is covered in the next section ...
just bear with me for now!

If we look at `journal.txt` we'll see it's our very first version. And if we
look at our directory,

~~~
$ ls
~~~
{: .bash}
~~~
journal.txt
~~~
{: .output}

then we see that our `common` directory is gone. But, rest easy, while it's
gone from our working directory, it's still in our repository. We can jump back
to the latest commit by doing:

~~~
$ git checkout master
~~~
{: .bash}

And `common` will be there once more,

~~~
$ ls
~~~
{: .bash}
~~~
common journal.txt
~~~
{: .output}
So we can get any version of our files from any point in time. In other words,
we can set up our working directory back to any stage it was when we made
a commit.

### The *HEAD* and *master* pointers

*HEAD* is essentially a pointer which points to the branch at the commit where
you currently are. 
We said previously that *master* is the default branch. But *master* is
actually a pointer - that points to the tip of the master branch (the sequence
of commits that is created by default by Git). You may think of *master* as two
things: one as a pointer and one as the default branch. 

Before we checked out one of the past commits, the *HEAD* pointer was pointing to
*master* i.e. the most recent commit of the master branch. 
After checking out one of the past commits, *HEAD* was pointing to that commit i.e.
not pointing to master any more. 
That is what Git means by a 'detached HEAD' state and advises us that if we want to make a commit
now, we should create a new branch to retain these commits. 

![Checking out a previous commit - detached head](../fig/detached_head.svg)

If we created a new commit without first creating a new branch, i.e. working from
the 'detached HEAD' these commits would not overwrite any of our existing work,
but they would not belong to any branch. In order to save this work, we would need
to checkout a new branch. To discard any changes we make from the detached HEAD
state, we can just checkout master again.

### Using tags as nicknames for commit identifiers

Commit identifiers are long and cryptic. Git allows us to create tags, which
act as easy-to-remember nicknames for commit identifiers.

For example,

```    
$ git tag PAPER_STUB
```
{: .bash}

We can list tags by doing:

```    
$ git tag
```
{: .bash}

Now add the second paper to references.txt and commit the change:

```    
$ gedit common/references.txt	# Add second paper
$ git add references.txt 
$ git commit -m "Add Jones et al paper" references.txt
```
{: .bash}

We can checkout our previous version using our tag instead of a commit
identifier.

```    
$ git checkout PAPER_STUB
```
{: .bash}

And return to the latest checkout,

```    
$ git checkout master
```
{: .bash}

> ## Top tip: tag significant events 
> When do you tag? Well, whenever you might want to get back to the exact
> version you've been working on. For a paper, this might be a version that has
> been submitted to an internal review, or has been submitted to a conference.
> For code this might be when it's been submitted to review, or has been
> released.
{: .callout}

> ## Where to create a Git repository?
> Avoid creating a Git repository within another Git repository.
> Nesting repositories in this way causes the 'outer' repository to
> track the contents of the 'inner' repository - things will get confusing!
{: .callout}

> ## Exercise: "bio" Repository 
>
> - Create a new Git repository on your computer called "bio"
> - Be sure not to create your new repo within the 'papers' repo (see above)
> - Write a three-line biography for yourself in a file called **me.txt**
> - Commit your changes
> - Modify one line, add a fourth line, then save the file
> - Display the differences between the updated file and the original
>
> > ## Solution
> > 
> > ```
> > cd ..                # Navigate out of the papers directory
> >                      # Avoid creating a repo within a repo - confusion will arise!
> > mkdir bio            # Create a new directory
> > cd bio               # Navigate into the new directory
> > git init             # Initialise a new repository
> > gedit me.txt         # Create a file and write your biography
> > git add me.txt       # Add your biography file to the staging area
> > git commit           # Commit your staged changes
> > gedit me.txt         # Edit your file
> > git diff me.txt      # Display differences between your modified file and the last committed version
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

