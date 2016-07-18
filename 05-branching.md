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

You might have noticed the term `branch` in status messages:

~~~{.bash}
$ git status journal.txt 
~~~
~~~{.output}
On branch master 
nothing to commit (working directory clean)
~~~

and when we wanted to get back to our most recent version of the repository, we
used `git checkout master`.

Not only can our repository store the changes made to files and directories, it
can store multiple sets of these, which we can use and edit and update in
parallel. Each of these sets, or parallel instances, is termed a `branch` and
`master` is Git's default branch. 

 A new branch can be created from any commit. Branches can also be merged
 together. 

 Why is this useful? Suppose we've developed some software and now we want to
 add some new features to it but we're not sure yet whether we'll keep them. We
 can then create a branch 'feature1' and keep our master branch clean. When
 we're done developing the feature and we are sure that we want to include it
 in our program, we can merge the feature branch with the master branch. 
 
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
developing. The merged branch is not deleted. We can continue developing (and
making commits) in feature1 as well.

```    
-c1---c2---c3---c5---c6---c7--c10---c11--c12     master
       \                      / 
       c4---c8---c9---------------c13            feature1
```
One popular model is to have:

- A release branch, representing a released version of the code
- A master branch, representing the most up-to-date stable version of the
code
- Various feature and/or developer-specific branches representing
work-in-progress, new features etc

For example:

```
      0.1        0.2    0.3 
      c6---------c9-----c17------                release
      /         /       / 
c1---c2---c3--c7--c8---c16--c18---c20---c21--    master
|                     /
c4---c10---c13------c15                          fred 
|                   / 
c5---c11---c12---c14---c19                       kate
```

There are different possible workflows when using Git for code development. 

One of the examples may be when the master branch holds stable and tested code.
If a bug is found by a user, a bug fix can be applied to the release branch,
and then merged with the master branch.  When a feature or developer-specific
branch, is stable and has been reviewed and tested it can be merged with the
master branch. When the master branch has been reviewed and tested and is ready
for release, a new release branch can be created from it.  If you want to learn
more about workflows with Git, have a look at the [AstroPy development
workflow](http://astropy.readthedocs.org/en/latest/development/workflow/development_workflow.html).


### Branching in practice

One of our colleagues wants to contribute to the paper but is not quite sure
if it will actually make a publication. So it will be safer to create a branch
and carry on working on this "experimental" version of the paper in a branch
rather than in the master.

~~~{.bash}
$ git checkout -b paperWJohn 
Switched to a new branch 'paperWJohn'
~~~

Now let's change the title of our paper and the authors (adding John Smith).
Let's commit our changes. Before we do that, it's a good practice to check
whether we're working in the correct branch.

~~~{.bash}
$ git branch 
~~~
~~~{.output}
  master
* paperWJohn 
~~~

The * indicates which branch we're currently in. Let's commit. If we want to
work now in our master branch. We can switch by using:

~~~{.bash}
$ git checkout master 
~~~
~~~{.output}
Switched to branch 'master'
~~~

### Merging and resolving conflicts

We are now working on two papers. Our main one in our master branch and the one
which may possibly be collaborative work in our "paperWJohn" branch. Let's
suppose that we have a new idea for the title for our main paper. We can change
it in our master branch. Let's do it and commit changes.

~~~{.bash}
$ gedit journal.txt ......  
$ git add journal.txt 
$ git commit -m "Change title" journal.txt
~~~

After some discussions with John we decided that there is going to be a 
change to our plan. We will publish together. And hence it makes sense now to
merge all that was authored together with John in branch "paperWJohn". 

 We can do that by *merging* that branch with the master branch. Let's try
 doing that:

~~~{.bash}
$ git merge paperWJohn 
~~~
~~~{.output}
Auto-merging journal.txt
CONFLICT (content): Merge conflict in journal.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~

Git cannot complete the merge because there is a conflict - if you recall,
journal.txt differs in the same places (lines) in the master and the paperWJohn
branch. We have to resolve the conflict and then complete the merge. We can get
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

- Keep the local version, which, here, is the one marked-up by HEAD i.e.
"Laboratory measurements of atmospheric particle formation"

- Keep the remote version, which, here, is the one marked-up by paperWJohn
i.e. "Simulations of atmospheric particle formation"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "Measurement-model comparison of atmospheric particle formation in laboratory experiements"

We edit the file. Then commit our changes:

~~~{.bash}
$ git add journal.txt 
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
