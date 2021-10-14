---
layout: page
title: Setup
permalink: /setup/
---
## Files
There are no files required for this lesson. You will create all the files needed.

## GitHub account
You will need a GitHub account to follow this course.
[Sign up][GitHub] if you don't already have an account.


## Software
Git is free and open-source software, available for all operating systems.
The PCs in the training rooms and clusters at the University Of Manchester should already have Git installed.
If you are using Windows to follow this course, you should open `Git BASH`.

### Installation on a managed desktop PC
[Git for Windows](https://git-for-windows.github.io/) is available for self-installation from the
[Software Centre](https://supportcentre.manchester.ac.uk/ServiceDesk.WebAccess/wd/object/open.rails?class_name=Knowledge.Article&key=2713eff4-2720-4db8-a1f6-a4bbb0d70cab).

### Installation on personal and unmanaged machines
[See here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for installation instructions
for the major operating systems.

## SSH keys
Please [set up SSH keys] for authentication with GitHub.
After following the linked instructions, you can verify it was successful:

~~~
$ ssh -T git@github.com
~~~
{: .language-bash}

~~~
Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
~~~
{: .output}

Please get in touch before the course starts if you run into any problems with set up.

[GitHub]: https://github.com/
[set up SSH keys]: ./_episodes/08-remote/#set-up-ssh-keys