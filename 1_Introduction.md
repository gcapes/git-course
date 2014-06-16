title: Version control with Git  
---

**Based on materials by Mike Jackson, Greg Wilson, Chris Cannam, Katy
Huff, Anthony Scopatz, Joshua R. Smith, and Sri Hari Krishna
Narayanan. All materials created originally for [Software Carpentrty](http://www.software-carpentry.orgs)**

Introduction: What is version control?
--------------------------------------

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

Outline
--------
1. Setting up and working with a local repository.
2. Looking at differences.
3. Discarding changes, undoing things.
4. Travelling back in time.
5. Branching, merging and resolving conflicts.
6. Rebasing - introduction.
7. Setting up and working with remote repositories.
8. Collaborative work over a remote repository (including Pull Requests).
9. Tips&tricks and resources for learning Git further.
10. Summary and Q&A.s

Agenda
-----
**10:00 - 10:30** Introduction to version control

**10:30 - 12:00** Setting up and working with local repository (including:
diffs, undoing things, looking back at history etc.)

**12:00 - 13:00** Lunch

**13:00 - 14:30** Branching (creating branches and working with them);
merging with and without conflicts; a bit of internals of Git
(pointers, commits, branches); intro to rebasing

**14:30 - 15:00** Setting up a remote repository and working with it
(commit-push-pull-clone etc.); few words about GitHub

**15:00 - 15:30** Break 

**15:30 - 16:15** Collaborating in pairs/groups on remote repository in
GutHub - introduction to the Push-Pull workflow; getting others' changes
into your local repository; resolving conflicts 

**16:15 -17:00** Pull Requests in GitHub - collaboration without having
write permits to a repo/contributing to large projects


Next: [Creating and working with local repository](1_Local.md)