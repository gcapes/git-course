---
title: 'Version control with Git and GitHub'
author: 'Gerard Capes'
---

# Version control with Git and GitHub
1. Please sit near the front
2. Open the notes at [http://gcapes.github.io/git-course/](http://gcapes.github.io/git-course/)
3. Sign up for a [GitHub account](https://github.com/)
4. Attendance is recorded using a [feedback form]; there is no sign-in sheet
5. You can use Windows or Linux on the training PCs

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

[Training courses]: http://www.staffnet.manchester.ac.uk/staff-learning-and-development/academicandresearch/practical-skills-and-knowledge/it-skills/research-computing/research-courses/

# Housekeeping
- Fire exit
- Toilets
- Course timing
	- 09:30 -- 12:00 Morning session
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
- Sticky notes
	- Used for getting help and giving real-time feedback
	- Green: :), OK, ready to continue
	- Red: :( (too fast, don't understand, computer says no etc)
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

## Checking out a previous commit: "detached HEAD"
![](../fig/detached-head.svg)

---

# Exercise 1
## Create your own repo

[http://gcapes.github.io/git-course/03-history](http://gcapes.github.io/git-course/03-history/#exercise-bio-repository)

---

# Branches
![](../fig/feature-branch.svg)

---

# 3 Exercises on Branching
[http://gcapes.github.io/git-course/05-branching/](http://gcapes.github.io/git-course/05-branching/#add-a-commit-to-detached-head)

---

# Revert
### Create a new commit

![](../fig/git-revert.svg)

# Reset
### Delete commit(s)

![](../fig/git-reset.svg)

# Exercises on remote collaboration
[https://gcapes.github.io/git-course/09-remote-collaboration](http://gcapes.github.io/git-course/09-remote-collaboration/#collaborating-on-a-remote-repository)

&nbsp;

## [Feedback form][feedback form] = attendance record


# Merge vs rebase
![](../fig/slideshow/forked-history.svg)
