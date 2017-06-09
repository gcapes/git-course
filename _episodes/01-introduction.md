---
title: "Introduction"
teaching: 15
exercises: 0
questions:
- "Why use version control?"
objectives:
- "Understand the benefits of an automated version control system."
keypoints:
- "Store versions neatly."
- "Restore previous versions."
- "Understand what happened and why."
- "Always know which is the current version."
---

## What is a version control system?

Version control is a piece of software which allows you to record and
preserve the history of changes made to directories and files. If you
mess things up, you can retrieve an earlier version of your project.

## Why use a version control system?

![[Piled Higher and Deeper by Jorge Cham, http://www.phdcomics.com](http://www.phdcomics.com)](../fig/phd101212s.png)

The comic above illustrates some of pitfalls of working without version
control. Some of the benefits are given below:

## Storing versions (properly)
Saving files after you have made changes should be an automatic habit.
However if you want to have different versions of your code, you will
need to save the new version somewhere else or with a different name.

* Do you just save the file(s) you changed, or all the files in the project?
* How do you name these different versions? It is very easy to lose track
of what is what.
* How do you know what is different between each version?

Without a VCS you will probably end up with lots of nearly-identical
(but critically different) copies of the same file, which is confusing
and wastes hard drive space.

A VCS treats your files as one project, so you only have one current
version on your disk (the working copy) - all the other variants and
previous versions are saved in the VCS repository. A VCS starts with
a base version of your project and only saves the changes you make along
the way, so it is much more space efficient too.

Add changes sequentially
![Add changes sequentially](../fig/play-changes.svg)

Save different versions
![Save different versions](../fig/versions.svg)

Merge different versions
![Merge different versions](../fig/merge.svg)

## Restoring previous versions
The ability to restore previous versions of a file (or all the files
in your project) greatly reduces the scope for screw ups. If you make
changes which you later want to abandon (e.g. the wording of your
conclusion section was better before you started making changes, your 
code changes end up breaking things which previously worked and you
can't figure out why etc), you can just undo them by restoring a previous
version.

## Understanding what happened
Each time you save a new version of your project, VCS requires you to
give a description of why you made the changes. This helps identify
which version is which.

## Backup
For distributed version control like Git, each person working on the
project has a complete copy of the project's  history (i.e. the repository)
on their hard drive. This acts as a backup for the server hosting the
remote repository.

## Collaboration
Without VCS, you are probably using a shared drive and taking turns to
edit files, or emailing files back and forth. This makes it really
easy to overwrite or abandon someone else's changes because you have
to manually incorporate the other person's changes into your version
and vice versa.

With VCS, everyone is able to work on any file at any time without
affecting anyone else. The VCS will then help you merge all the changes
into a common version. It is also always clear where the most recent
version is kept (in the repository).


## Example scenario
Think about the following situation:

You are working on a handful of MATLAB files. You make a few changes,
and then you want to try something you're not quite confident about
yet, so you save a copy in another folder just in case.

Then you want to try out the program with more data on a bigger machine,
and you make a few changes there to get it working properly. Then you 
try out something else in the copy on your laptop.

Now you have three or four copies, all slightly different, and you have
some results generated from all of them, and you include some of it in
a paper.

Then someone asks for the same results based on a new data file. You have 
to go off and remind yourself which version you used, find out whether
you still have it at all or whether you've changed it again since, check 
whether it really has the vital changes you thought you'd included but
that might have been only on that other machine, and so on.

You should easily be able to see the benefits of VCS in the situation above.

## What files can I track using version control?
VCS is typically used for software source code, but it can be used for
any kind of text[^1] file: 

- Configuration files
- Parameter sets
- Data files
- User documentation, manuals, and journal papers,  whether they be plain-text,
LaTeX, XML, md etc
- Have a look at some of the projects on [GitHub](https://github.com/explore)

For this session, we'll be using Git, a popular distributed version control system
and [GitHub](http://github.com), a web-based service providing remote
repositories. *Distributed* means that each user has a complete copy of
the repository on their computer and can commit changes offline. If you 
have used a centralized version control system before e.g. Subversion,
this will be one of the major differences to how you are used to working.
See [here](https://git.wiki.kernel.org/index.php/GitSvnComparsion) for a more 
detailed comparison of Git and Subversion.


Next: [Tracking changes using a local repository]({{page.root }}/02-local/)

[^1]: VCS can be used for binary (non-text) files, but viewing differences
between versions becomes problematic.
