## Tracking your changes with a local repository

Version control is centred round the notion of a *repository* which holds your
directories and files. We'll start by looking at a local repository. The local
repository is set up in a directory in your local filesystem (local machine).

### Create a new repository with Git

We will be working with a simple example in this tutorial. It will be a paper
that we will first start writing as a single author and then work on it further
with one of our colleagues. 

 First, let's create a directory:

     
       $ mkdir papers $ cd papers

Now, we need to set up this directory up to be a Git repository (or "initiate
the repository"):

       $ git init Initialized empty Git repository in /home/user/papers/.git/

The directory "papers" is now our working directory. 

 If we look in this directory, we'll find a *.git* directory:

      $ ls -a .git: branches  config  description  HEAD  hooks  info  objects
      refs

.git directory contains Git's configuration files. Be careful not to
accidentally delete this directory!

### Tell Git who we are

As part of the information about changes made to files Git records who made
those changes. In teamwork this information is often crucial (do you want to
know who rewrote your 'Conclusions' section?). So, we need to tell Git about
who we are:  

    $ git config --global user.name "Your Name" $ git config --global
    user.email "yourname@yourplace.org"

### Set a default editor

When working with Git we will often need to provide some short but useful
information. In order to enter this information we need an editor. We'll now
tell Git which editor we want to be the default one (i.e. Git will always bring
it up whenever it wants us to provide some information).

You can choose any available in you system editor. For the purpose of this
session we'll use *nano*:

    $ git config --global core.editor nano

To set up vi as the default editor:

    $ git config --global core.editor vi

To set up xemacs as the default editor:

    $ git config --global core.editor xemacs
    
### Git's global configuration

We can now preview (and edit, if necessary) Git's global configuration (such as
our name and the default editor which we just set up). If we look in our home
directory, we'll see a `.gitconfig` file,

    $ cat ~/.gitconfig [user] name = Your Name email = yourname@yourplace.org
    [core] editor = nano

This file holds global configuration that is applied to any Git repository in
your file system. 

### Add a file to the repository

Now, we'll create a file. Let's say we're going to write a journal paper:

    $ nano journal.txt

and add headings for Title, Author, Introduction, Conclusion and References,
and save the file.\ git status allows us to find out about the current status
of files in the repository. So, we can run,

    $ git status journal.txt

Information about what Git knows about the file is displayed. For now, the
important bit of information is that our file is listed as Untracked which
means it's in our working directory but Git is not tracking it - that is, any
changes made to this file will not be recorded by Git. To tell Git about the
file, we will use the *add* command:

    $ git add journal.txt $ git status journal.txt
    
Now, our file is now listed as one of some Changes to be committed. 
    
*git add* is used for two purposes. Firstly, to tell Git that a given file
should be tracked. Secondly, to put the file into the Git's **staging area**
which is also known as the index or the cache. 

The staging area can be viewed as a "loading dock", a place to hold files we've
added, or changed, until we're ready to tell Git to record those changes in the
repository. 

In order to tell Git to record our change, our new file, into the repository,
we need to  *commit* it:

    $ git commit

Our default editor will now pop up. Why? Well, Git can automatically figure out
that directories and files are committed, and who by (thanks to the information
we provided before) and even, what changes were made, but it cannot figure out
why. So we need to provide this in a commit message. So let's type in
a message. "Initial structure and headings for the journal paper." 



 Ideally, commit messages should have meaning to others who may read them - or
 you 6 months from now. Messages like "made a change" or "added changes" or
 "commit 5" aren't that helpful (in fact, they're redundant!). A good commit
 message usually contains a one-line description followed by a longer
 explanation, if necessary. 

 If we save our commit message, Git will now commit our file.

     [master (root-commit) c381e68] This is my journal paper.  1 file changed,
     9 insertions(+) create mode 100644 journal.txt


This output shows the number of files changed and the number of lines inserted
or deleted across all those files. Here, we've changed (by adding) 1 file and
inserted 9 lines.

Now, if we look at its status,

    $ git status journal.txt # On branch master nothing to commit (working
    directory clean)

nothing to commit means that our file is now in the repository, our working
directory is up-to-date and we have no uncommitted changes in our staging area. 


Let's work a bit further on our `journal.txt` file adding a few lines to
different sections.
   

 Now let's make some more changes to our journal.txt file . If we now run,

     $ git status journal.txt

we see changes not staged for commit section and our file is marked as
modified. This means that a file Git knows about has been modified by us but
has not yet been committed. So we can add it to the staging area and then
commit the changes:

     
    $ git add journal.txt $ git commit
    
Note that in this case we used "git add" to put journal.txt to the staging
area. Git already knows this file should be tracked but doesn't know if we want
to commit the changes we made to the file  in the repository and hence we have
to add the file to the staging area. 


It can sometimes be quicker to provide our commit messages at the command-line
by doing:

    $ git add journal.txt $ git commit -m "Expanded introduction."
    
    
To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

    $ git log

The output shows: the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit, author, date, and your
comment.     

> **Top tip: When to commit changes**

> There are no hard and fast rules, but good commits are atomic - they are the
smallest change that remain meaningful.  > For code, it's useful to commit
changes that can be reviewed by someone else in under an hour. 

> **What we know about software development - code reviews work** 

> Fagan (1976) discovered that a rigorous inspection can remove 60-90% of
errors before the first test is run.  > M.E., Fagan (1976). [Design and Code
inspections to reduce errors in program
development](http://www.mfagan.com/pdfs/ibmfagan.pdf). IBM Systems Journal 15
(3): pp. 182-211.

> **What we know about software development - code reviews should be about 60
minutes long** 

> Cohen (2006) discovered that all the value of a code review comes within the
first hour, after which reviewers can become exhausted and the issues they find
become ever more trivial.  > J. Cohen (2006). [Best Kept Secrets of Peer Code
Review](http://smartbear.com/SmartBear/media/pdfs/best-kept-secrets-of-peer-code-review.pdf).
SmartBear, 2006. ISBN-10: 1599160676. ISBN-13: 978-1599160672.

Let's add a directory, common and a file references.txt for references we may
want to reuse:

    $ mkdir common $ nano common/references.txt
    
We will also add a few lines to our paper (journal.txt). Now we need to record
our work in the repository so we need to make a commit.  First we tell Git to
track the references. We can actually tell Git to track everything in the given
subdirectory:

    $ git add common

All files that are in "common" are now tracked.  We would also have to add
journal.txt to the staging area. But there is a shortcut. We can use option
"-a" for "commit". This option means "commit all files that are tracked and
that have been modified".

    $ git commit -am "Added common directory and references file with Cohen et
    al reference and described data in the paper"

and Git will add, then commit, both the directory and the file.

> **Top tip: Commit anything that cannot be automatically recreated**

> Typically we use version control to save anything that we create manually
e.g. source code, scripts, notes, plain-text documents, LaTeX documents.
Anything that we create using a compiler or a tool e.g. object files (`.o`,
`.a`, `.class`, `.pdf`, `.dvi` etc), binaries (`exe` files), libraries (`dll`
or `jar` files) we don't save as we can recreate it from the source. Adopting
this approach also means there's no risk of the auto-generated files becoming
out of synch with the manual ones.

In order to add all tracked files to the staging area (which may be very useful
when you edited, let's say, 10 files and now you want to commit all of them):

    $ git commit -a



### Looking at differences

Let us suppose we've made a change to our file and not yet committed it. We can
see the changes that we've made:

    $ git diff journal.txt

This shows the difference between the latest copy in the repository and the
changes we've made. 

* `-` means a line was deleted.  * `+` means a line was added.  * Note that,
a line that has been edited is shown as a removal of the old line and an
addition of the updated line.

Looking at differences between commits is one of the most common activities.
The `diff` command itself has a number of [useful
options](http://git-scm.com/docs/git-diff.html).

There is also a more elaborate front end to the `diff` command - [`git
difftool`](). `git difftool` is used for comparing and editing files that
changed between commits using common diff tools. There is a range of such
tools, including emerge, kompare, meld, and vimdiff.

There is a range of GUI-based tools supporting looking at differences and
editing files. For example:

* [WinMerge](http://winmerge.org/) - open source tool available for Windows;
* GitHub [Compare
view](https://help.github.com/articles/comparing-commits-across-time)


The choice of GUI for viewing differences depends on the context in which you
are working and your own preferences related to choosing tools and
technologies.



### Looking at our history

To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

    $ git log

The output shows: the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit, author, date, and your
comment. *git log* command has many options to print information in various
ways, for example:

    $ git log --relative-date


Git automatically assigns an identifier (*COMMITID*) to each commit made to the
repository. In order to see the changes made between any earlier commit and our
current version, we can use  `git diff`  providing the commit identifier of the
earlier commit:

    $ git diff COMMITID

And, to see changes between two commits:

    $ git diff OLDER_COMMITID NEWER_COMMITID

Using our commit identifiers we can set our working directory to contain the
state of the repository as it was at any commit. So, let's go back to the very
first commit we made,

    $ git log $ git checkout COMMITID


We will get something like this:

    Note: checking out 'c4354a9c578aa5b81d354d8b3330fda7b9b23d3e'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:
    git checkout -b new_branch_name

    HEAD is now at c4354a9... Added sections

*HEAD* is essentially a pointer which points to the branch where you currently
are. We said previously that *master* is the default branch. But *master* is
actually a pointer - that points to the tip of the master branch (the sequence
of commits that is created by default by Git). You may think of *master* as two
things: one as a pointer and one as the default branch. 

When we checked out one of the past commits HEAD is pointing to that commit but
does not point to the same thing as master any more. That is why git says You
are in 'detached HEAD' state and advises us that if we want to make a commit
now, we should create a new branch to retain these commits. 

If we created a new commit without creating a new branch Git would not know
what to do with it (since there is already a commit in master branch from the
current state which we checked out c4354a...). We will get back to branches and
HEAD pointer later in this tutorial. 



If we look at `journal.txt` we'll see it's our very first version. And if we
look at our directory,

    $ ls journal.txt

then we see that our `references` directory is gone. But, rest easy, while it's
gone from our working directory, it's still in our repository. We can jump back
to the latest commit by doing:

    $ git checkout master

And `references` will be there once more,

    $ ls references journal.txt

So we can get any version of our files from any point in time. In other words,
we can set up our working directory back to any stage it was when we made
a commit.

> **Top tip: Commit often**

> In the same way that it is wise to frequently save a document that you are
working on, so too is it wise to save numerous revisions of your files. More
frequent commits increase the granularity of your "undo" button.

While DropBox and GoogleDrive also preserve every version, they delete old
versions after 30 days, or, for GoogleDrive, 100 revisions. DropBox allows for
old versions to be stored for longer but you have to pay for this. Using
revision control the only bound is how much space you have!

### Using tags as nicknames for commit identifiers

Commit identifiers are long and cryptic. Git allows us to create tags, which
act as easy-to-remember nicknames for commit identifiers.

For example,

    $ git tag PAPER_STUB

We can list tags by doing:

    $ git tag

Now if we change our file,

    $ git add journal.txt $ git commit -m "..." journal.txt

We can checkout our previous version using our tag instead of a commit
identifier.

    $ git checkout PAPER_STUB

And return to the latest checkout,

    $ git checkout master

> **Top tip: tag significant "events"**

> When do you tag? Well, whenever you might want to get back to the exact
version you've been working on. For a paper this, might be a version that has
been submitted to an internal review, or has been submitted to a conference.
For code this might be when it's been submitted to review, or has been
released.



Previous: [Introduction](1_Introduction.md) Next: [Branching](3_Branching.md)

