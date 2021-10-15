---
title: "Git hints and tips"
teaching: 15
exercises: 0
questions:
- "How can I find help?"
- "How can I further customise Git to suit my preferences?"
objectives:
- "Access help online and from command prompt"
- "Configure Git to ignore certain types of files"
- "Understand how to build a commit selectively using `git add --patch`"
keypoints:
- "List files to ignore by committing a `.gitignore` file"
- "Selectively stage changes to files using `git add --patch`"
- "Configure a visual diff tool and use `git difftool` to view changes"
---

### Getting help

#### `man` page

Like many Unix/Linux commands, `git` has a `man` page,

```
$ man git
```

You can scroll the manual page up and down using the up and down arrows.

You can search for keywords by typing `/` followed by the search term e.g. if
interested in help, type `/help` and then hit enter.

To exit the manual page, type `q`.


#### Command-line help

Type,

```
$ git --help
```
{: .language-bash}

and Git gives a list of commands it is able to help with, as well as their
descriptions.

You can get more help on a specific command, by providing the command name e.g.

```
$ git init --help
$ git commit --help
```
{: .language-bash}

#### Google
Search for your problem online. Someone has probably already asked (and answered) your question
on stackoverflow.com.

---

### Ignore scratch, temporary and binary files

You can create a `.gitignore` file which lists the patterns of files you want
Git to ignore. It's common practice to not add to a repository any file you can
automatically create in some way e.g. C object files (`.o`), Java class
(`.class`) files or temporary files e.g. XEmacs scratch files (`~`). Adding
these to `.gitignore` means Git won't complain about them being untracked.

Create or edit `gitignore`,

```
$ gedit .gitignore
```
{: .language-bash}

Then add patterns for the files you want to ignore, where `*` is a wildcard,

    *~ *.o *.so *.dll *.exe *.class *.jar

Then, add `.gitignore` to your repository,

```
$ git add .gitignore $ git commit -m "Added rules to ignore vim scratch
files and binary files"
```
{: .language-bash}

---

### `git add --patch`
This is a way to stage only parts of a file. If you have done lots of work
without committing, it may be useful to commit your changes as a series of
small commits. This command allows you to choose which changes go into which
commit so you can group the changes logically.
- [Guide to `git add --patch`](
http://nuclearsquid.com/writings/git-add/)
- Manually [editing hunks] is the  most difficult aspect.

---

### `git commit --author`
You can commit changes made by someone else, by using the `--author`
flag. Consider how this may enable you to collaborate with your colleagues.
The syntax is:

`git add --author="FirstName Surname <Firstname.Surname@example.com>"`

---

### Colours in Git

On many computers, the terminal output is automatically coloured which makes
reading the output easier.
If your output is not coloured (e.g. in the Sackville/G11 cluster) there is a command
which will add the colour (**note the spelling of *color***):

```
$ git config --global --add color.ui true                       # Note US spelling of color
```
{: .language-bash}

---

### Add colour to `diff`

```
$ git config --global color.diff auto
```
{: .language-bash}

---

### Configure a visual diff tool

`git diff` is ok, but not very user friendly.
It represents changes as removal of a line, followed by the addition of a new line.
There are many diff GUIs available, which can be much easier to work with.
To view differences with a GUI instead of using the command-line diff tool, first configure
git to use your chosen diff tool:

```
$ git config --global diff.tool diffmerge    # Set diffmerge as your visual diff tool
$ git config --global difftool.prompt false  # Suppress confirmation before launching GUI
```
{: .language-bash}
Then to use the GUI, use the following command instead of `git diff`:

```
$ git difftool
```
{: .language-bash}

---

### `git stash`
Sometimes you are working on one branch and want to switch to another branch for
a while.
In order to do so you would normally need to have a clean working directory i.e.
no modified files or staged changes.
You could commit all the changes you have made, then switch branch, but that would
involve committing incomplete work just to return to this state later on.
`git stash` saves the dirty state of your working directory and saves it on a stack
of unfinished changes that you can reapply at any time using `git stash apply`.
See [here](https://git-scm.com/book/en/v1/Git-Tools-Stashing) for more details and
for examples.

---

### Password manager/ssh-agent
You can have the ssh-agent manage your password for you so you don't have to
keep entering it.

Start the ssh-agent

~~~
$ eval `ssh-agent`
~~~
{: .language-bash}

Add your key(s) to the agent:

~~~
$ ssh-add
~~~
{: .language-bash}

---

### Git GUIs

There are a number of available GUIs for working with Git. The official Git
page contains a [comprehensive list](http://git-scm.com/downloads/guis).

However, Git [for Windows](https://git-for-windows.github.io/) already comes
with all the tools you need (Git Bash, Git GUI, Shell integration).

Some IDEs already have integration with version control e.g. MATLAB, R studio.

---

### Git configuration

The global configuration file for git `.gitconfig` is automatically created by
Git in the `home` directory. If you set up some basic configuration (in the
first steps of this tutorial), it should look like this.

```
$ cat ~/.gitconfig
```
{: .language-bash}

```
[user]
	name = Your Name 
	email = yourname@yourplace.org
[core] 
	editor = gedit
```
{: .output}

You can add more configuration options. For example, instead of typing `git
commit -m` we can have a shorter version of this command:

```
$ git config --global alias.cms 'commit -m'
```
{: .language-bash}

And now our configuration file will have a new section added:

```
… [alias] cms = commit -m
```

Next time we can simply type:

```
$ git cms "Commit message"
```
{: .language-bash}

---

### Completely removing unwanted files from the repository

As we discussed earlier, there are a number of ways to undo what we did in Git.
However, most of the time, we actually want to make some amendments rather than
discard everything completely. Also often undoing things means, in fact,
creating a new commit (not abandoning them). Since Git is a version control
system, everything that we recorded in the past commits will be available in
the repository.

For example, if you accidentaly commited a file with sensitive data (passwords)
in your local repository and then pushed it to the remote repository, the file
will be there even if in the next commit-and-push you'll remove it (`git rm`).

This [article](https://help.github.com/articles/remove-sensitive-data) provides
a step-by-step tutorial on how to remove completely files from your repository
(purge the repository) using `git filter-branch`.

Removing files from the repository may be useful not only when the files
contain sensitive data. Another case may be if you commited a large file in
your local repository. Essentially, by default, there are no limitations on the
size of files you can commit. However, there may be (and quite likely there
will be) limits on the size of the files you can push to remote repositories
(GitHub allows for max 100MB). You may encounter an annoying situation when you
commited a large file locally and then kept on working making local commits but
not pushing. Finally, you decide to push to GitHub (or elsewhere remote) and
you can't because the file is too big. Using `git rm` won't help because you
are pushing since the last pushed commit and that means in between there is
a commit with the large problematic file. To recover from this you will have to
purge your large file from the repo (or switch to a different remote repo
provider that allows for large files).

Again, as always with Git **before** you execute the above, make sure you know
what you're doing!

[editing hunks]: http://joaquin.windmuller.ca/2011/11/16/selectively-select-changes-to-commit-with-git-or-imma-edit-your-hunk

{% include links.md %}
