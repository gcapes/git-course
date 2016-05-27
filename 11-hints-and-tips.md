---
layout: page
title: Version control with Git  
subtitle: Git hints and tips
---

### `man` page

Like many Unix/Linux commands, `git` has a `man` page,

```{.bash}
$ man git
```

You can scroll the manual page up and down using the up and down arrows.

You can search for keywords by typing `/` followed by the search term e.g. if
interested in help, type `/help` and then hit enter.

To exit the manual page, type `q`.

---

### Command-line help

Type,

```{.bash}
$ git --help
```

and Git gives a list of commands it is able to help with, as well as their
descriptions. 

You can get more help on a specific command, by providing the command name e.g.

```{.bash}
$ git init --help $ git commit --help
```

---

### Add a repository description

You can edit the file `.git/description` and give your repository a name e.g.
"My first repository".

---

### Ignore scratch, temporary and binary files

You can create a `.gitignore` file which lists the patterns of files you want
Git to ignore. It's common practice to not add to a repository any file you can
automatically create in some way e.g. C object files (`.o`), Java class
(`.class`) files or temporary files e.g. XEmacs scratch files (`~`). Adding
these to `.gitignore` means Git won't complain about them being untracked.

Create or edit `gitignore`,

```{.bash}
$ nano .gitignore
```

Then add patterns for the files you want to ignore, where `*` is a wildcard,

    *~ *.o *.so *.dll *.exe *.class *.jar

Then, add `.gitignore` to your repository,

```{.bash}
$ git add .gitignore $ git commit -m "Added rules to ignore vim scratch
files and binary files"
```

---

### Add colour to `diff`

```{.bash}
$ git config --global color.diff auto
```
    
 ---

### Git configuration

The global configuration file for git `.gitconfig` is automatically created by
Git in the `home` directory. If you set up some basic configuration (in the
first steps of this tutorial), it should look like this.

```{.bash}
$ cat ~/.gitconfig 
```
```{.output}
[user] name = Your Name email = yourname@yourplace.org 
[core] editor = nano
```
     	
You can add more configuration options. For example, instead of typing `git
commit -m` we can have a shorter version of this command:

```{.bash}
$ git config --global alias.cms 'commit -m'
```

And now our configuration file will have a new section added:
	
```
… [alias] cms = commit -m
```

Next time we can simply type:
	
```{.bash}
$ git cms "Commit message"
```
	
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
your local repository. Essentially, by deafult, there are no limitations on the
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

---

### Git GUIs

There are a number of available GUIs for working with Git. The official Git
page contains a [comprehensive list](http://git-scm.com/downloads/guis).

See for example: 

* [SourceTreeApp](http://www.sourcetreeapp.com/)

---

### `git add --patch`
This is a way to stage only parts of a file. If you have done lots of work
without committing, it may be useful to commit your changes as a series of
small commits. This command allows you to choose which changes go into which
commit so you can group the changes logically. See [this discussion](
http://nuclearsquid.com/writings/git-add/) for more information.

Previous: [Pull Requests](07-pullrequests.html) Next:
[Conclusion](09-conclusion.html)
