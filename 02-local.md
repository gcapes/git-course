---
layout: page
title: Version control with Git  
subtitle: Tracking changes with a local repository
---

> ## Learning objectives {.objectives}
> * Know how to set up a new Git repository
> * Understand how to start tracking files
> * Be able to commit changes to your repository

Version control is centred round the notion of a *repository* which holds your
directories and files. We'll start by looking at a local repository. The local
repository is set up in a directory in your local filesystem (local machine).

### Setting up Git
#### Create a new repository with Git

We will be working with a simple example in this tutorial. It will be a paper
that we will first start writing as a single author and then work on it further
with one of our colleagues. 

 First, let's create a directory within your home directory:

```{.bash}
$ cd           # Switch to your home directory.
$ pwd          # Print working directory (output should be /home/<username>)
$ mkdir papers 
$ cd papers
```

Now, we need to set up this directory up to be a Git repository (or "initiate
the repository"):

~~~{.bash}       
$ git init 
~~~
~~~{.output}
Initialized empty Git repository in /home/user/papers/.git/
~~~

The directory "papers" is now our working directory. 

 If we look in this directory, we'll find a *.git* directory:

~~~{.bash}
$ ls -a .git
~~~
~~~{.output}
branches  config  description  HEAD  hooks  info  objects refs
~~~
.git directory contains Git's configuration files. Be careful not to
accidentally delete this directory!

#### Tell Git who we are

As part of the information about changes made to files Git records who made
those changes. In teamwork this information is often crucial (do you want to
know who rewrote your 'Conclusions' section?). So, we need to tell Git about
who we are:  

~~~{.bash}
$ git config --global user.name "Your Name" 
$ git config --global user.email "yourname@yourplace.org"
~~~

#### Set a default editor

When working with Git we will often need to provide some short but useful
information. In order to enter this information we need an editor. We'll now
tell Git which editor we want to be the default one (i.e. Git will always bring
it up whenever it wants us to provide some information).

You can choose any editor available on your system. For the purpose of this
session we'll use *gedit*:

~~~{.bash}
$ git config --global core.editor gedit
~~~

To set up vi as the default editor:

~~~{.bash}
$ git config --global core.editor vi
~~~

To set up xemacs as the default editor:

~~~{.bash}
$ git config --global core.editor xemacs
~~~    

#### Git's global configuration

We can now preview (and edit, if necessary) Git's global configuration (such as
our name and the default editor which we just set up). If we look in our home
directory, we'll see a `.gitconfig` file,

~~~{.bash}
$ cat ~/.gitconfig 
    [user] name = Your Name email = yourname@yourplace.org
    [core] editor = gedit
~~~

This file holds global configuration that is applied to any Git repository in
your file system. 

---

### Tracking files with a git repository

Now, we'll create a file. Let's say we're going to write a journal paper:

~~~{.bash}
$ gedit journal.txt
~~~

and add headings for Title, Author, Introduction, Conclusion and References,
and save the file. git status allows us to find out about the current status
of files in the repository. So we can run,

~~~{.bash}
$ git status
~~~
~~~{.output}
On branch master

Initial commit

Untracked files:
(use "git add <file>..." to include in what will be committed)

journal.txt

nothing added to commit but untracked files present (use "git add" to track)
~~~

Information about what Git knows about the directory is displayed. We are on
the **master** branch, which is the default branch in a Git respository
(one way to think of branches is like parallel versions of the project - more
on branches later).

For now, the important bit of information is that our file is listed as
**Untracked** which means it's in our working directory but Git is not
tracking it - that is, any changes made to this file will not be recorded by
Git.

#### Add files to a Git repository
To tell Git about the file, we will use the *add* command:

~~~{.bash}
$ git add journal.txt 
$ git status journal.txt
~~~
~~~{.output}
On branch master

Initial commit

Changes to be committed:
(use "git rm --cached <file>..." to unstage)

      	new file:   journal.txt
~~~

Now, our file is now listed as one of some Changes to be committed. 
    
*git add* is used for two purposes. Firstly, to tell Git that a given file
should be tracked. Secondly, to put the file into the Git's **staging area**
which is also known as the index or the cache. 

The staging area can be viewed as a "loading dock", a place to hold files we've
added, or changed, until we're ready to tell Git to record those changes in the
repository. 

![The staging area](fig/git-staging-area.svg)

#### Commit changes

In order to tell Git to record our change, our new file, into the repository,
we need to  *commit* it:

~~~{.bash}
$ git commit
~~~

Our default editor will now pop up. Why? Well, Git can automatically figure out
that directories and files are committed, and who by (thanks to the information
we provided before) and even, what changes were made, but it cannot figure out
why. So we need to provide this in a commit message. So let's type in
a message. "Initial structure and headings for the journal paper" 

If we save our commit message, Git will now commit our file.

~~~{.output}
[master (root-commit) 21cfbde] Add initial structure and headings
1 file changed, 9 insertions(+)
create mode 100644 journal.txt
~~~

This output shows the number of files changed and the number of lines inserted
or deleted across all those files. Here, we've changed (by adding) 1 file and
inserted 9 lines.

Now, if we look at its status,

~~~{.bash}
$ git status
~~~
~~~{.output}
On branch master
nothing to commit, working directory clean
~~~

Nothing to commit means that our file is now in the repository, our working
directory is up-to-date and we have no uncommitted changes in our staging area. 

Let's work a bit further on our `journal.txt` file adding a few lines to
different sections.
   
If we now run,

~~~{.bash}
$ git status
~~~

we see changes not staged for commit section and our file is marked as
modified: 

~~~{.output}
On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

     modified:   journal.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~

This means that a file Git knows about has been modified by us but
has not yet been committed. So we can add it to the staging area and then
commit the changes:
     
~~~{.bash}
$ git add journal.txt 
$ git commit
~~~    
Note that in this case we used `git add` to put journal.txt to the staging
area. Git already knows this file should be tracked but doesn't know if we want
to commit the changes we made to the file  in the repository and hence we have
to add the file to the staging area. 


It can sometimes be quicker to provide our commit messages at the command-line
by doing:

~~~{.bash}
$ git commit -m "Add notes on what to include in introduction"
~~~    
Let's add a directory *common* and a file *references.txt* for references we may
want to reuse:

~~~{.bash}
$ mkdir common 
$ gedit common/references.txt
~~~    
We will also add a few lines to our paper (journal.txt). Now we need to record
our work in the repository so we need to make a commit.  First we tell Git to
track the references. We can actually tell Git to track everything in the given
subdirectory:

~~~{.bash}
$ git add common
~~~

All files that are in *common* are now tracked.  We would also have to add
journal.txt to the staging area. But there is a shortcut. We can use option
"-a" for `commit`. This option means "commit all files that are tracked and
that have been modified".

~~~{.bash}
$ git commit -am "Add common directory and references file"
~~~
and Git will add, then commit, both the directory and the file.

In order to add all tracked files to the staging area (which may be very useful
when you edited, let's say, 10 files and now you want to commit all of them):

~~~{.bash}
$ git commit -a
~~~

![The Git commit workflow](fig/git-committing.svg)

Previous: [Introduction](01-introduction.html) Next: [Looking at history and differences](03-history.html)

