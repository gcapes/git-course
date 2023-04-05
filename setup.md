---
title: Setup
---

## Files

There are no files required for this lesson. You will create all the files needed.

## GitHub account

You will need a GitHub account to follow this course.
[Sign up][GitHub] if you don't already have an account.

## Software

### Git

Git is free and open-source software, available for all operating systems.

#### Installation on a managed desktop PC

[Git for Windows](https://git-for-windows.github.io/) is available for self-installation from the
[Software Centre](https://manchester.saasiteu.com/Modules/SelfService/#knowledgeBase/view/19D08D7414AE4D85998B2F79EC4C4B99).

#### Installation on personal and unmanaged machines

[See here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for installation instructions
for the major operating systems.

### SSH keys

Please set up SSH keys for authentication with GitHub.

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

```
$ ls ~/.ssh
```
{: .language-bash}

If you already have ssh keys set up, your output will look something like this:

```
id_ed25519  id_ed25519.pub
```
{: .output}

and you can jump to the [final step](#add-public-ssh-key-to-github).

If you still need to set up ssh keys, you'll get a message like this:

```
ls: cannot access '/home/yourusername/.ssh': No such file or directory
```
{: .output}

To set up the key pair, we use the following command

```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
{: .language-bash}

You *might* get an error from this if your system doesn't support
the ed25519 algorithm, in which case you can try `$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/you/.ssh/id_ed25519):
```
{: .output}

Accept the default option using Enter.

```
Created directory  '/home/you/.ssh'.
Enter passphrase (empty for no passphrase):
```
{: .output}

Enter a password (you'll be prompted to enter it twice)

```
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
```
{: .output}

Now that we have generated the SSH keys, we will find the SSH files when we check.

```
$ ls ~/.ssh
```
{: .language-bash}

```
id_ed25519  id_ed25519.pub
```
{: .output}

We can view the public key using

```
$ cat ~/.ssh/id_ed25519.pub
```
{: .language-bash}

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI your_email@example.com
```
{: .output}

Now you should copy the output from this command ready for the final step.


#### Add public ssh key to GitHub

The final step is to add the public key to our GitHub accounts.

- On [GitHub], click on your profile icon in the top right corner
- Click “Settings,” then on the settings page
- Click “SSH and GPG keys”
- Click the “New SSH key” button on the right side.
- Add a title e.g. "my_work_laptop" and paste your SSH key into
  the field, and click on “Add SSH key” to complete the setup.

You can verify your SSH set up was successful like this:

```
$ ssh -T git@github.com
```
{: .language-bash}

```
Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
```
{: .output}

### Set the default GitHub branch name to 'master'

As we will see in [episode 2], the default branch name in a git repo is *master*.

In 2021 GitHub and many other remote repo providers changed their settings
so that new repositories will use *main* instead of master.
As ever there are arguments [for] and [against] this change.
We can however choose the default branch name in our GitHub settings,
so let's set it to master to be consistent with the git software itself.

On GitHub, click on your profile photo at the top right of the page.
Then go to Settings -> Repositories -> Repository default branch.

Change 'main' to 'master' and click 'update'.

**Please get in touch before the course starts if you run into any problems with set up!**

[GitHub]: https://github.com/
[for]: https://www.zdnet.com/article/github-to-replace-master-with-alternative-term-to-avoid-slavery-references/
[against]: https://dev.to/dandv/8-problems-with-replacing-master-in-git-2hck
[SSH]: https://en.wikipedia.org/wiki/Secure_Shell_Protocol
[episode 2]: {{ page.root }}{% link _episodes/02-local.md %}
