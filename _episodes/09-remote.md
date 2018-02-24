---
title: "Working from multiple locations with a remote repository"
teaching: 25
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
GitHub **isn't** the only remote repostitories provider. It is however very popular, 
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

### Create a new repository

Now, we can create a repository on GitHub,

* Log in to [GitHub](https://GitHub.com/) 
* Click on the **Create** icon on the top right 
* Enter Repository name: "papers"
* For the purpose of this exercise we'll create a public repository 
* Make sure that **Initialize this repository with a README** is **unselected** 
* Click **Create Repository**

You'll get a page with new information about your repository. We already have
our local repository and we will be *pushing* it to GitHub, so this is the
option we will use:

```
$ git remote add origin https://github.com/<USERNAME>/papers.git 
$ git push -u origin master
```
{: .bash}

The first line sets up an alias `origin`, to correspond to the URL of our
new repository on GitHub.


### Push locally tracked files to a remote repository

Now copy and paste the second line,

```
$ git push -u origin master 
```
{: .bash}
```
Counting objects: 32, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (28/28), done.
Writing objects: 100% (32/32), 3.29 KiB | 0 bytes/s, done.
Total 32 (delta 7), reused 0 (delta 0)
To https://github.com/gcapes/papers.git
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
{: .bash}
    
The branch should now be created in our GitHub repository.    

To list all branches (local and remote):

```
$ git branch -a
```
{: .bash}
    
> ## Deleting branches (for information only) 
> **Don't do this now.** This is just for information.
> To delete branches, use the following syntax:
>
> ```
> $ git branch -d <branch_name>			# For local branches
> $ git push origin --delete <branch_name>	# For remote branches
> ```
> {: .bash}
{: .callout}

### Cloning a remote repository

Now, let's do something drastic! (but before that step, **make sure that you
pushed all your local branches into the remote repository**)

```
$ cd .. 
$ rm -rf papers
```
{: .bash}

Gulp! We've just wiped our local repository! But, as we've a copy on GitHub we
can just copy, or `git clone` that,

```
$ git clone https://github.com/<USERNAME>/papers.git 
```
{: .bash}
```
Cloning into 'papers'...
remote: Counting objects: 32, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 32 (delta 7), reused 32 (delta 7), pack-reused 0
Unpacking objects: 100% (32/32), done.
Checking connectivity... done.
```
{: .output}

Cloning creates an exact copy of the repository. By deafult it creates
a directory with the same name as the name of the repository. 

Now, if we change into *papers* we can see that we have our repository,

```    
$ cd papers 
$ git log
```
{: .bash}
and we can see our Git configuration files too:

```    
$ ls -A
```
{: .bash}

In order to see the other branches locally, we can check them out as before:

```
$ git branch -r					# Show remote branches
$ git checkout results				# Check out the results branch
```
{: .bash}

### Push changes to a remote repository

We can use our cloned repository just as if it was a local repository so let's
make some changes to our files and commit these.

```
$ git checkout master				# We'll continue working on the master branch
$ gedit journal.md				# Add section which will contain the figures 
$ git add journal.md				# Add figures section
$ git commit
```
{: .bash}

Having done that, how do we send our changes back to the remote repository? We
can do this by *pushing* our changes,

```
$ git push origin master
```
{: .bash}

If we now check our GitHub page we should be able to see our new changes under
the *Commit* tab.

To see all remote repositories (we can have multiple ones!) type:
	
```
$ git remote -v
```
{: .bash}
{: .bash}
