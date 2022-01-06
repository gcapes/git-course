---
title: "Working from multiple locations with a remote repository"
teaching: 30
exercises: 0
questions:
- "What is a remote repository"
- "How can I use GitHub to work from multiple locations?"
objectives:
- "Understand how to set up remote repository"
- "Understand how to push local changes to a remote repository"
- "Understand how to clone a remote repository"
keypoints:
- "Git is the version control system: GitHub is a remote repositories provider."
- "`git clone` to make a local copy of a remote repository"
- "`git push` to send local changes to remote repository"
---

We're going to set up a remote repository that we can use from multiple
locations. The remote repository can also be shared with colleagues, if we want
to.

### GitHub

[GitHub](http://GitHub.com) is a company which provides remote repositories for
Git and a range of functionalities supporting their use. GitHub allows users to
set up  their private and public source code Git repositories. It provides
tools for browsing, collaborating on and documenting code. GitHub, like other
services such as [Launchpad](https://launchpad.net),
[Bitbucket](https://bitbucket.org), [GoogleCode](http://code.google.com), and
[SourceForge](http://sourceforge.net) supports a wealth of resources to support
projects including:

* Time histories changes to repositories
* Commit-triggered e-mails
* Browsing code from within a web browser, with syntax highlighting
* Software release management
* Issue (ticket) and bug tracking
* Download
* Varying permissions for various groups of users
* Other service hooks e.g. to Twitter.

**Note**  GitHub's free repositories have public licences **by default**. If
you don't want to share (in the most liberal sense) your stuff with the world
and you want to use GitHub, you will need to pay for the
private GitHub repositories (GitHub offers up to 5 free private repositories,
if you are an academic - but do check this information as T&C may change).

### GitHub for research
GitHub **isn't** the only remote repositories provider. It is however very popular,
in particular within the Open Source communities. The reason why we teach GitHub
in this tutorial is mainly due to popular demand.

Also, GitHub has started working on [functionality which is particularily useful
for researchers](https://github.com/blog/1840-improving-github-for-sciences)
such as making code citable.

---

### Get an account

Let's get back to our tutorial. We will first need a GitHub account.

[Sign up](https://GitHub.com) or if you already have an account [sign
in](https://GitHub.com).


### Set up SSH keys
[SSH] is an encrypted network protocol which we will use to securely access
our remote repository.
In order to use it, we need to set up a pair of SSH keys,
which are used together to validate access.
There's a private key, and a public key - GitHub needs to know the public key, but the private
key stays only on your computer.
A useful analogy is to think of the public key as a padlock,
and the private key as the only key to the padlock.

#### Create ssh keys

Let's first check whether we already have ssh keys set up:

~~~
$ ls ~/.ssh
~~~
{: .language-bash}

If you already have ssh keys set up, your output will look something like this:

~~~
id_ed25519  id_ed25519.pub
~~~
{: .output}

and you can jump to the [final step](#add-public-ssh-key-to-github).

If you still need to set up ssh keys, you'll get a message like this:

~~~
ls: cannot access '/home/yourusername/.ssh': No such file or directory
~~~
{: .output}

To set up the key pair, we use the following command
~~~
$ ssh-keygen -t ed25519 -C "your_email@example.com"
~~~
{: .language-bash}

You *might* get an error from this if your system doesn't support
the ed25519 algorithm, in which case you can try
`$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

~~~
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/you/.ssh/id_ed25519):
~~~
{: .output}

Accept the default option using <kbd>Enter</kbd>.

~~~
Created directory  '/home/you/.ssh'.
Enter passphrase (empty for no passphrase):
~~~
{: .output}

Enter a password (you'll be prompted to enter it twice)

~~~
Your identification has been saved in /home/you/.ssh/id_ed25519
Your public key has been saved in /home/you/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:SMSPIStNyA00KPxuYu94KpZgRAYjgt9g4BA4kFy3g1o your_email@example.com
The key's randomart image is:
+--[ED25519 256]--+
|^B== o.          |
|%*=.*.+          |
|+=.E =.+         |
| .=.+.o..        |
|....  . S        |
|.+ o             |
|+ =              |
|.o.o             |
|oo+.             |
+----[SHA256]-----+
~~~
{: .output}

Now that we have generated the SSH keys, we will find the SSH files when we check.

~~~
$ ls ~/.ssh
~~~
{: .language-bash}

~~~
id_ed25519  id_ed25519.pub
~~~
{: .output}

We can view the public key using

~~~
$ cat ~/.ssh/id_ed25519.pub
~~~
{: .language-bash}

~~~
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI your_email@example.com
~~~
{: .output}

Now you should copy the output from this command ready for the final step.

#### Add public ssh key to GitHub

The final step is to add the public key to our GitHub accounts.
- On [GitHub], click on your profile icon in the top right corner
- Click “Settings,” then on the settings page
- Click “SSH and GPG keys”
- Click the “New SSH key” button on the right side.
- Add a title e.g. "my_work_laptop" and paste your SSH key into
  the field, and click the “Add SSH key” to complete the setup.

### Set the default GitHub branch name to 'master'
As we saw in [episode 2], the default branch name in a git repo is *master*.

In 2021 GitHub and many other remote repo providers changed their settings
so that new repositories will use *main* instead of master.
As ever there are arguments [for] and [against] this change.
We can however choose the default branch name in our GitHub settings,
so let's set it to master to be consistent with the git software itself.

On GitHub, click on your profile photo at the top right of the page.
Then go to Settings -> Repositories -> Repository default branch.

Change 'main' to 'master' and click 'update'.


### Create a new repository

Now, we can create a repository on GitHub,

* Log in to [GitHub]
* Click on the **Create** icon on the top right
* Enter Repository name: "paper"
* For the purpose of this exercise we'll create a public repository
* Make sure that **Initialize this repository with a README** is **unselected**
* Click **Create Repository**

You'll get a page with new information about your repository. We already have
our local repository and we will be *pushing* it to GitHub using SSH,
so this is the option we will use:

```
$ git remote add origin git@github.com:<USERNAME>/paper.git
$ git push -u origin master
```
{: .language-bash}

The first line sets up an alias `origin`, to correspond to the URL of our
new repository on GitHub.


### Push locally tracked files to a remote repository

Now copy and paste the second line,

```
$ git push -u origin master
```
{: .language-bash}
```
Counting objects: 32, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (28/28), done.
Writing objects: 100% (32/32), 3.29 KiB | 0 bytes/s, done.
Total 32 (delta 7), reused 0 (delta 0)
To https://github.com/gcapes/paper.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```
{: .output}

This **pushes** our `master` branch to the remote repository, named via the alias
`origin` and creates a new `master` branch in the remote repository.

Now, on GitHub, we should see our code and if we click the `Commits` tab we should see
our complete history of commits.

Our local repository is now available on GitHub. So, anywhere we can access
GitHub, we can access our repository.


### Push other local branches to a remote repository

Let's push each of our local branches into our remote repository:

```
$ git push origin branch_name
```
{: .language-bash}

The branch should now be created in our GitHub repository.

To list all branches (local and remote):

```
$ git branch -a
```
{: .language-bash}

> ## Deleting branches (for information only)
> **Don't do this now.** This is just for information.
> To delete branches, use the following syntax:
>
> ```
> $ git branch -d <branch_name>			# For local branches
> $ git push origin --delete <branch_name>	# For remote branches
> ```
> {: .language-bash}
{: .callout}

### Cloning a remote repository

Now that we have a copy of the repo on GitHub,
we can download or `git clone` a fresh copy to work on from another computer.

So let's pretend that the repo we've been working on so far is on a PC in the office,
and you want to do some work on your laptop at home in the evening.

Before we clone the repo, we'll navigate up one directory so that we're not already in a git repo.
```
cd ..
```
{: .language-bash}

Then to clone the repo into a new directory called `laptop_paper`

```
$ git clone https://github.com/<USERNAME>/paper.git laptop_paper
```
{: .language-bash}
```
Cloning into 'laptop_paper'...
remote: Counting objects: 32, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 32 (delta 7), reused 32 (delta 7), pack-reused 0
Unpacking objects: 100% (32/32), done.
Checking connectivity... done.
```
{: .output}

Cloning creates an exact copy of the repository. By deafult it creates
a directory with the same name as the name of the repository.
However, we already have a `paper` dircectory,
so have specified that we want to clone into a new directory `laptop_paper`.

Now, if we `cd` into *laptop_paper* we can see that we have our repository,

```
$ cd laptop_paper
$ git log
```
{: .language-bash}
and we can see our Git configuration files too:

```
$ ls -A
```
{: .language-bash}

In order to see the other branches locally, we can check them out as before:

```
$ git branch -r					# Show remote branches
$ git checkout simulations			# Check out the simulations branch
```
{: .language-bash}

### Push changes to a remote repository

We can use our cloned repository just as if it was a local repository so let's
[add a results section][add-results] and commit the changes.

```
$ git checkout master				# We'll continue working on the master branch
$ gedit paper.md				# Add results section
$ git add paper.md				# Stage changes
$ git commit
```
{: .language-bash}

Having done that, how do we send our changes back to the remote repository? We
can do this by *pushing* our changes,

```
$ git push origin master
```
{: .language-bash}

If we now check our GitHub page we should be able to see our new changes under
the *Commit* tab.

To see all remote repositories (we can have multiple!) type:

```
$ git remote -v
```
{: .language-bash}

[GitHub]: https://github.com/
[add-results]: https://github.com/gcapes/git-course-paper/commit/0c4573e5ea15d6f5dc877e8db8c0696e7675d5ed
[SSH]: https://en.wikipedia.org/wiki/Secure_Shell_Protocol
[for]: https://www.zdnet.com/article/github-to-replace-master-with-alternative-term-to-avoid-slavery-references/
[against]: https://dev.to/dandv/8-problems-with-replacing-master-in-git-2hck
[episode 2]: {{ page.root }}{% link _episodes/02-local.md %}

{% include links.md %}
