---
layout: page
title: Version control with Git  
subtitle: Introduction - What is version control?
---

**Based on materials by [Software Carpentry](http://www.software-carpentry.org)**

## Introduction: What is version control?

> ## Learning Objectives {.objectives}
>
> *   Understand the benefits of an automated version control system.

> ## Prerequisites {.prereq}
>
> In this lesson we use Git from the Unix Shell.
> Some previous experience with the shell is expected,
> *but isn't mandatory*.


Version control is a piece of software which allows you to record and
preserve the history of changes made to directories and files. If you're
familiar with Track Changes in Microsoft Word, or the versions of files
saved in DropBox or GoogleDrive, then you've already used a simple form
of version control.

Think about the following situations:

-   Someone asks you, "Can I have the code you used to create the data
    that you graphed in your conference paper?". How easy would it be
    for you to get this code for them?
-   Someone tells you, "Your laptop's just been stolen!". How much work
    have you lost?
-   You're working with a colleague on a journal paper who storms into
    your office and shouts, "You've just deleted my analysis section".
    Would you have to ask them to write it again?
-   You're developing a piece of software with your colleagues. You find
    that a function you wrote has been rewritten and you want to know
    why, how easy would it be to find this out? Also, you can't exactly
    remember the implementation details and you accidentaly deleted the
    copy of the source code you had on your laptop. You would still like
    to use your original code. What would you do?

Version control, also known as revision control, helps with all of these
problems. It is typically used for software source code, but it can also
be used for:

-   Configuration files.
-   Parameter sets.
-   Data files.
-   User documentation, manuals, conference papers, journal papers, book
    chapters, whether they be plain-text, LaTeX, XML, or whatever.

There are different types of version control. They may be divided into
two types: centralized and distributed version control. An example of
the former is Subversion (SVN) and of the latter is
[Git](http://git-scm.com/). Centralized version control has one main
repository (on a server) whilst in the case of distributed version
control everyone has their own (local) repository. 

 For this session, we'll be using Git, a popular version control system
and [GitHub](http://github.com), a web-based service providing remote
repositories.

Next: [Creating and working with a local repository](02-local.html)
