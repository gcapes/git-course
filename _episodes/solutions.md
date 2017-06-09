---
layout: page
title: Version control with Git  
subtitle: Solutions to exercises
---

> ## Bio repository {.challenge}
> 
> ```{.bash}
> cd ..                # Navigate out of the papers directory
>                      # Avoid creating a repo within a repo - confusion will arise!
> mkdir bio            # Create a new directory
> cd bio               # Navigate into the new directory
> git init             # Initialise a new repository
> gedit me.txt         # Create a file and write your biography
> git add me.txt       # Add your biography file to the staging area
> git commit           # Commit your staged changes
> gedit me.txt         # Edit your file
> git diff me.txt      # Display differences between your modified file and the last committed version
> ```

> ## Branching {.challenge}
> ####1. Detached head
>
> ```{.bash}
> git checkout HEAD~1  # Check out the commit one before last
> gedit journal.txt    # Make some edits
> git add journal.txt  # Stage the changes
> git commit           # Commit the changes
> git branch           # You should see a message like the one below,
>                      # indicating your commit does not belong to a branch
> ```
> ```{.output}
> * (detached from 57289fb)
>   master
> ```
>
>
> ####2. Save changes to a new branch
> 
> ```{.bash}
> git checkout master
> ```
>
> The output you see will be slightly different to that below,
> reflecting your previous commit message and commit ID.
>
> ```{.output}
> Warning: you are leaving 1 commit behind, not connected to
> any of your branches:
>
> eb7c650 Add empty line for branching exercise
>
> If you want to keep them by creating a new branch, this may be a good time
> to do so with:
>
>  git branch new_branch_name eb7c650
>
>  Switched to branch 'master'
>  Your branch is up-to-date with 'master'.
> ```
>
> We will ignore this message for now and discard the changes.
> Make some new changes so you can follow the process coming from a clean working directory.
> This is effectively the start of the exercise - we have just been clearing up so far.
>
> ```{.bash}
> gedit journal.txt		# Modify the paper, but don't stage or commit the changes yet
> git branch exercise		# Create a new branch
> git checkout exercise		# Switch to the new branch 
> ```
>
> You should see similar output to below. 
> This means you have created a new branch, starting from the previous commit you checked out.
> The 'M' next to the file name indicates this file has modifications.
> You can now stage, then commit your changes.
>
> ```{.output}
> M	journal.txt
> Switched to branch 'exercise'
> ```
> ```{.bash}
> git add journal.txt	# Stage your modified file
> git commit			# Commit your staged changes
> git checkout master	# Switch back to the 'master' branch
> ```
