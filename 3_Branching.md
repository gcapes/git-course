### Branching

####What is a branch?

You might have noticed the term `branch` in status messages,

    $ git status add_numb.py
    # On branch master
    nothing to commit (working directory clean)

and when we wanted to get back to our most recent version of the repository, we used,

    $ git checkout master

Not only can our repository store the changes made to files and directories, it can store multiple sets of these, which we can use and edit and update in parallel. Each of these sets, or parallel instances, is termed a *branch* and `master` is Git's default branch. 

A new branch can be created from any commit. Branches can also be *merged* together. 

Why is this useful? 
Suppose we've developed some software and now we want to add some new features to it but we're not sure yet whether we'll keep them. We can then create a branch 'feature1' and keep our master branch clean. When we're done developing the feature and we are sure that we want to include it in our program, we can merge the feature branch with the master branch.

We create our branch for the new feature. 

    -o---o---o                               master
              \
               o                             feature1

We can then continue developing our software in our default, or master, branch,

    -o---o---o---o---o---o                   master
              \
               o                             feature1

And, we can work on the new feature in the feature1 branch

    -o---o---o---o---o                       master
              \
               o---o---o                     feature1

We can then merge the feature1 branch adding new feature to our master branch (main program):

    -o---o---o---o---o---M                   master
              \         /
               o---o---o                     feature1

And continue developing,

    -o---o---o---o---o---M---o---o           master
              \         /
               o---o---o                     feature1

One popular model is to have,

* A release branch, representing a released version of the code.
* A master branch, representing the most up-to-date stable version of the code.
* Various feature and/or developer-specific branches representing work-in-progress, new features etc.

For example,

             0.1      0.2        0.3
              o---------o------o------    release
             /         /      /
     o---o---o--o--o--o--o---o---o---    master
     |            /     /
     o---o---o---o     /                 fred
     |                /
     o---o---o---o---o                   kate

If a bug is found by a user, a bug fix can be applied to the release branch, and then merged with the master branch. When a feature or developer-specific branch, is stable and has been reviewed and tested it can be merged with the master branch. When the master branch has been reviewed and tested and is ready for release, a new release branch can be created from it.

Let's try it in practice.

We'll change our program creating a module containing two fuctions 'add_two' and 'add_three'. However, as we're not sure yet whether this is going to work, we'll be doing this in a separate feature branch.

    $ git checkout -b feature1
    Switched to a new branch 'feature1'

Let's now create a module and write a function which adds three numbers. The module will be called 'adding.py' and the code is:

    def add_three(a,b,c):
        return a+b+c

Now, let's modify our add_numb.py file:
    """ This short program just adds numbers. """

    from adding import add_three
    def add_two(a,b):
        return a+b
    def main():
        print ("2 + 3 = ",add_two(2,3))
        print ("2 + 3 + 4 = ",add_three(2,3,4))
    if __name__ == "__main__":
        main()


Let's commit our changes. Before we do that, it's a good practice to check whether we're working in the correct branch.

    $ git branch
    * feature1
      master

The * indicates which branch we're currently in. Let's commit. If we want to work now in our master branch. We can switch by using:

    $ git checkout master 
    Switched to branch 'master'

The module which we added in our feature branch is not in our master:

    $ ls
    add_numb.py doc

To list all (local) branches:

    $ git branch
    feature1
    * master

Git is very powerful and comes with countless number of commands. For example, if you want to see all branches with their commits use:

    $ git show-branch
    * [feature1] Moved add_two to the module
    ! [master] Added files
    --
    *  [feature1] Moved add_two to the module
    *  [feature1^] Adding module with functions
    *+ [master] Added files

Let's change the add_two function by adding a printing statement and commit our changes (remember: we're working now in the master branch!).
Now let's switch back to feature1 and move now our add_two function to the module and commit the changes. Looks like our feature is ready to be merged. To do that we need to switch to the master branch:

      $ git checkout master

Now merging:

    $ git merge feature1
    Auto-merging add_numb.py
    CONFLICT (content): Merge conflict in add_numb.py
    Automatic merge failed; fix conflicts and then commit the result.

Git cannot complete the merge because there is a conflict - if you recall our add_numb.py is different in our master branch and feature1 branch. We have to resolve the conflict and then complete the merge. Let's see a bit more details:

    $ git status
    # On branch master
    # You have unmerged paths.
    #   (fix conflicts and run "git commit")
    #
    # Changes to be committed:
    #
    # new file:   adding.py
    #
    # Unmerged paths:
    #   (use "git add <file>..." to mark resolution)
    #
    # both modified:      add_numb.py
    #

To resolve the conflicts we need to edit the add_numb.py:

    """ This short program just adds numbers. """
    <<<<<<< HEAD
    def add_two(a,b):
        print('I'm just about to add two numbers')
        return a+b
    =======
     from adding import add_three, add_two
    >>>>>>> feature1
    def main():
        print ("2 + 3 = ",add_two(2,3))
        print ("2 + 3 + 4 = ",add_three(2,3,4))
    if __name__ == "__main__":
            main()

The markers <<<<<<< and ======= show us where the conflict has occured (i.e. show different contents from each of the file versions). Since we want to use our feature we will delete the part marked to be HEAD.
Now we need to let Git know that we resolved out conflict by adding the file to the staging area and making a commit ('git commit -a' will also work but 'git commit' will return an error).

    $ git commit -am 'Resolved conflicts'
    [master 99aae93] Resolved conflicts

Now we have our feature in our master branch.

Our feature1 branch still exists and we could keep on working with it. But let's suppose we actually would prefer to delete it. 

    $ git branch -d feature1
    Deleted branch feature1 (was 2bce0f2).


Relative references
====
* ^
* ~


Branch forcing
====
Branches can be moved around. Branch can be reassigned to a particular commit using the following command (with the option "-f"):

* git branch -f <branch_name/pointer_name> <commit_ID/TAG/relative_ref>

**Questions**

*What are the possible issues which may result from reassigning branch to a particular commit?*

Looking at our history - revisited
====
We already looked at "giong back in time with Git". But now we'll look at it in more detail to see how moving back relates to branches and we will learn how to actually undo things. So far we were moving back in time in one branch by checking out one of the past commits. But then we were able to mainly 


Previous: [Working with local repository](2_Local.md) Next: [Undoing things](4_Undoing.md)




