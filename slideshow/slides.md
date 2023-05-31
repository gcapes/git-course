---
title: 'Version control with Git and GitHub'
author: 'Gerard Capes'
---

# Version control with Git and GitHub
1. Open the notes at [http://gcapes.github.io/git-course/](https://gcapes.github.io/git-course/)
2. Make sure you've completed all the setup steps at <https://gcapes.github.io/git-course/setup.html>


[feedback form]: https://goo.gl/forms/YZJ05PzX9tPFEtuV2

# Research-related IT services
- Described on [IT Services website](http://www.itservices.manchester.ac.uk/research/)
- Announcements given via [Research IT News](https://researchitnews.org/)
- [Training courses] teaching computing skills for Research
- General guidance and advice about research software
- Access to specialist support and consultancy e.g. code reviews
- Access to HPC systems
- [Full list of services on offer](http://www.itservices.manchester.ac.uk/our-services/research/)
- For help and support, use the [Support Portal](http://www.itservices.manchester.ac.uk/help/)

[Training courses]: https://www.staffnet.manchester.ac.uk/staff-learning-and-development/learning-pathways/professional-and-technical-development/it-skills/research-computing/research-courses/

# Course timing
- 09:00 -- 12:00 Morning session
- 12:00 -- 13:00 Lunch
- 13:00 -- 16:00 Afternoon session


# Teaching methods
- Interactive, workshop-style course
	- Code along with the examples
	- Test your understanding in the exercises
- Course notes
	- All examples and exercises are in the notes
	- Slides will remain online after the course

# Getting help
- Sticky notes/zoom reactions
	- Used for getting help and giving real-time feedback
	- Green = OK, ready to continue
	- Red = too fast, don't understand, computer says no etc
- Please interrupt me to ask questions
- Peer learning
	- During exercises, please help each other as required
	- Please try to be quiet during worked examples so everyone can hear

# What is a version control system?
- Version control is a piece of software which allows you to record and preserve the history of changes made to directories and files.
- If you mess things up, you can retrieve an earlier version of your project.

---

<img src="../fig/phd101212s.png" width="50%">

---

# Storing versions without VCS
- Save a copy elsewhere?
- Save with a different name?
- How do you name different versions?
- What's different between them?
- Many of copies of nearly-identical but critically different files.

---

![](../fig/astorytoldinfilenames.gif)

---

# Why use version control
## To store versions properly

- VCS treats files as one project - one current version on disk,
previous versions and variations are saved in a repository
- VCS starts with a base version of the project,
and only saves the subsequent changes you make
- In order to save a new revision, a commit message is required,
which explains why the changes were made.

---

## Changes are saved sequentially

![](../fig/play-changes.svg)

---

## Different versions can be saved
<p style="text-align:center;"><img src="../fig/versions.svg" alt="Drawing" style="width: 500px;"/></p>

---

## Multiple versions can be merged
<p style="text-align:center;"><img src="../fig/merge.svg" alt="Drawing" style="width: 500px;"/></p>


# Why use version control?
- Restore previous versions
- Understand what happened
- Backup
- Collaboration

# Before we get started
- Example scenario
- Text files vs binary files
- Git vs GitHub

# Open the notes

## [https://gcapes.github.io/git-course](https://gcapes.github.io/git-course)

---

## Why learn the commands?

![](https://imgs.xkcd.com/comics/git.png)

---

# The staging area
![](../fig/git-staging-area.svg)

---

## Switching to a previous commit: "detached HEAD"
![](../fig/detached-head.svg)

---

# Exercise 1
## Create your own repo

[http://gcapes.github.io/git-course/03-history](http://gcapes.github.io/git-course/03-history/#exercise-bio-repository)

---

# Branches
![](../fig/feature-branches.svg)

---

# Three exercises on branching
[http://gcapes.github.io/git-course/06-branching/](http://gcapes.github.io/git-course/06-branching/#add-a-commit-to-detached-head)

---

# Revert
### Create a new commit

![](../fig/git-revert.svg)

# Reset
### Delete commit(s)

![](../fig/git-reset.svg)

# Exercises on remote collaboration
[https://gcapes.github.io/git-course/10-remote-collaboration](http://gcapes.github.io/git-course/10-remote-collaboration/#collaborating-on-a-remote-repository)

&nbsp;

## [Feedback form][feedback form]

# Rebasing
![](../fig/git-rebase.svg)

# Merge vs rebase
![](../fig/forked-history.svg)

# Standard merge
![](../fig/merge-without-rebase.svg)

# Rebase onto master
![](../fig/rebase-master.svg)

# (FF) merge after rebase
![](../fig/rebase-then-merge.svg)

# Forks and Pull Requests
![](../fig/github-diagram.png)

# Send me a PR!
[https://gcapes.github.io/git-course/13-pull-requests/#send-me-a-pull-request](https://gcapes.github.io/git-course/13-pull-requests/#send-me-a-pull-request)
