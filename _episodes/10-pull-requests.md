---
title: "Pull Requests"
teaching: 20
exercises: 15
questions:
- "How can I contribute to a repository to which I don't have write access?"
objectives:
- "Understand what it means to fork a repository"
- "Be able to fork a repository on GitHub"
- "Understand how to submit a pull request"
keypoints:
- "A fork is a `git clone` into your (GitHub) account"
- "A pull request asks the owner of a repository to incorporate your changes"
---

Pull Requests are a great solution for contributing to repositories to which
you don't have write access. Adding other people as *collaborators* to a remote
repository is a good idea but sometimes (or even most of the time) you want to
make sure that their contributions will provide more benefits that the
potential mistakes they may introduce.

In large projects, primarily Open Source ones, in which the community of
contributors can be very big, keeping the source code safe but at the same
allow people to make contributions without making them "pass" tests for their
skills and trustworthiness may be one of the keys to success. 

Leveraging the power of Git, GitHub provides a functionality called *Pull
Requests*. Esentially it's "requesting the owner of the repository to pull in
your contributions". The owner may or may not accept them. But for you as
a contributor, it was really easy to make the contribution.

The process looks like this:

- Find a repository on GitHub that belongs to someone else
- **Fork** it (`git clone` it on GitHub's servers into your GitHub account)
- `git clone` it to your PC/laptop
- Make changes, and push them to your repository on GitHub
- Request that the owner of the repository you *forked* pulls in your changes

![Conceptual illustration of a pull request - image adapted from [here](http://acrl.ala.org/techconnect/post/coding-collaboration-on-github)](../fig/github-diagram.png)

> ## Exercise 
> Let's look at the workflow and try to repeat it:
> 
> 1. **Fork** [this
> repository](https://github.com/gcapes/manchester-papers.git).  
> 2. Clone the repository from **YOUR** GitHub account. When you run `git remote -v`
> you should get something like this:
> 	
> ```{.output}
> origin	https://github.com/YOUR_USERNAME/manchester-papers.git(fetch)
> origin 	https://github.com/YOUR_USERNAME/manchester-papers.git(push)
> ```
> 
> 3. Make changes you want to contribute. Commit and push them back to your
> repository. You won't be able to push back to the repository you forked from
> because you are not added as a contributor!
> 4. Go to your GitHub account and in the forked repository find a green button
> for creating Pull Requests. Click it and follow the instructions.
> 5. The owner of the original repository gets a notification that someone 
> created a pull request - the request can be reviewed, commented and merged in 
> (or not) via GitHub.
{: .challenge}
