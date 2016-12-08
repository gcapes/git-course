---
layout: page
title: Version control with Git  
subtitle: Solutions to exercises
---

> ## Bio repository {.challenge}
> 
> ```{.bash}
> cd			# navigate out of the papers directory. 
>      			# Avoid creating a repo within a repo - confusion will arise!
> mkdir bio		# create a new directory
> cd bio		# navigate into the new directory
> git init		# initialise a new repository
> gedit bio.txt		# create a file and write your biography
> git add bio.txt	# add your biography file to the staging area
> git commit 		# commit your staged changes
> gedit bio.tx		# edit your file
> git diff bio.txt	# display differences between your modified file and the last committed version
> ```
