---
layout: page
title: Version control with Git 
subtitle: Branching
---

> ## Learning objectives {.objectives}
> * Know what branches are and why you would use them
> * Understand how to merge branches
> * Understand how to resolve conflicts during a merge

### What is a branch?

You might have noticed the term *branch* in status messages:

~~~{.bash}
$ git status 
~~~
~~~{.output}
On branch master 
nothing to commit (working directory clean)
~~~

and when we wanted to get back to our most recent version of the repository, we
used `git checkout master`.

Not only can our repository store the changes made to files and directories, it
can store multiple sets of these, which we can use and edit and update in
parallel. Each of these sets, or parallel instances, is termed a **branch** and
*master* is Git's default branch. 

 A new branch can be created from any commit. Branches can also be *merged*
 together. 

 Why is this useful? Suppose we've developed some software and now we want to
 try out some new ideas but we're not sure yet whether we'll keep them. We
 can then create a branch 'feature1' and keep our master branch clean. When
 we're done developing the feature and we are sure that we want to include it
 in our program, we can merge the feature branch with the master branch. 
 This keeps all the work-in-progress separate from the master branch, which 
 contains tested, working code.
 
 We create our branch for the new feature.

```    
-c1---c2---c3        master 
       \
       c4            feature1
```

We can then continue developing our software in our default, or master, branch,

```     
-c1---c2---c3---c5---c6---c7    master
       \ 
       c4                       feature1
```
And, we can work on the new feature in the feature1 branch

```    
-c1---c2---c3---c5---c6---c7         master 
       \
       c4---c8---c9                  feature1
```

We can then merge the feature1 branch adding new feature to our master branch
(main program):

```
-c1---c2---c3---c5---c6---c7--c10       master
       \                      /
       c4---c8---c9----------           feature1

```
When we merge our feature1 branch with master git creates a new commit which
contains merged files from master and feature1. After the merge we can continue
developing. **The merged branch is not deleted.** We can continue developing (and
making commits) in feature1 as well.

```    
-c1---c2---c3---c5---c6---c7--c10---c11--c12     master
       \                      / 
       c4---c8---c9---------------c13            feature1
```
One popular model is to have:

- A master branch, representing a released version of the code
- A release branch, representing the beginnings of the next release - a branch 
where the code is still undergoing testing
- Various feature and/or developer-specific branches representing
work-in-progress, new features etc

For example:

![Git workflow [(image from http://nurelm.com/our-workflow-git-flow/)](http://nurelm.com/our-workflow-git-flow/)](fig/git-flow.png)

There are different possible workflows when using Git for code development. 
If you want to learn more about workflows with Git, have a look at the
[feature branch workflow](http://nvie.com/posts/a-successful-git-branching-model/).


### Branching in practice

One of our colleagues wants to contribute to the paper but is not quite sure
if it will actually make a publication. So it will be safer to create a branch
and carry on working on this "experimental" version of the paper in a branch
rather than in the master.

~~~{.bash}
$ git checkout -b paperWJohn
~~~
~~~{.output}
Switched to a new branch 'paperWJohn'
~~~

We're going to change the title of the paper and update the author list (adding John Smith).
However, before we get started it's a good practice to check that we're working
on the right branch.

~~~{.bash}
$ git branch        # Double check which branch we are working on
~~~
~~~{.output}
  master
* paperWJohn 
~~~

The * indicates which branch we're currently in. Now let's make the changes to the paper.

~~~{.bash}
$ gedit journal.txt   # Change title and add co-author
$ git add journal.txt
$ git commit          # Modify title and add John as co-author
~~~

If we now want to work in our master branch. We can switch back by using:

~~~{.bash}
$ git checkout master 
~~~
~~~{.output}
Switched to branch 'master'
~~~

Having written some of the paper, we have thought of a better title for
the *master* version of the paper.

~~~{.bash}
$ gedit journal       # Rewrite the title
$ git add journal.txt
$ git commit          # Rewrite title emphasising measurements
~~~

### Merging and resolving conflicts

We are now working on two papers: the main one in our master branch and the one
which may possibly be collaborative work in our "paperWJohn" branch.
Let's add another section to the paper to write about John's simulations.

~~~{.bash}
$ git checkout paperWJohn       # Switch branch
$ gedit journal.txt             # Add 'simulations' section
$ git add journal.txt 
$ git commit -m "Add simulations" journal.txt
~~~

After some discussions with John we decided that we will publish together,
hence it makes sense to now merge all that was authored together with John 
in branch "paperWJohn". 

 We can do that by *merging* that branch with the master branch. Let's try
 doing that:

~~~{.bash}
$ git checkout master   # Switch branch
$ git merge paperWJohn  # Merge paperWJohn into master
~~~
~~~{.output}
Auto-merging journal.txt
CONFLICT (content): Merge conflict in journal.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~

Git cannot complete the merge because there is a conflict - if you recall,
after creating the new branch, we changed the title of the paper on both branches.
We have to resolve the conflict and then complete the merge. We can get
some more detail

~~~{.bash}
$ git status
~~~
~~~{.output}
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    	both modified:      journal.txt
~~~

Let's look inside journal.txt:

```
title
<<<<<<< HEAD
Laboratory measurements of atmospheric particle formation
=======
Simulations of atmospheric particle formation
>>>>>>> paperwjohn
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to resolve the
conflict. This means removing the mark-up and doing one of:

- Keep the local version, which is the one marked-up by HEAD i.e.
"Laboratory measurements of atmospheric particle formation"

- Keep the remote version, which is the one marked-up by paperWJohn
i.e. "Simulations of atmospheric particle formation"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "Measurement-model comparison of atmospheric particle formation in laboratory experiments"

We edit the file. Then commit our changes:

~~~{.bash}
$ gedit journal.txt    # Resolve conflict by editing journal.txt
$ git add journal.txt  # Let Git know we have resolved the conflict
$ git commit -m "Rewrite title to incorporate simulations"
~~~

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.


### Looking at our history - revisited
We already looked at "going back in time with Git". But now we'll look at it in
more detail to see how moving back relates to branches and we will learn how to
actually undo things. So far we were moving back in time in one branch by 
checking out one of the past commits. 

But we were then in the "detached HEAD" state.

> ###Exercise{.challenge}
> 1a. Add a commit to detached HEAD
>
> - Checkout one of the previous commits from our repository.
> - Make some changes and commit them. What happened?
> - Now try to run `git branch`. What can you see?
> 
> 1b. Commit changes to a new branch
>
> - Checkout the master branch again: `git checkout master` 
> - Make some changes and save the file(s). 
> - Create a new branch and check it out.
> - What happened?
> - Now commit the file to the new branch
> - Switch back to (i.e. checkout) the master branch 

Previous: [Committing changes](04-commit-advice.html) Next: [Undoing
changes](06-undoing.html)
