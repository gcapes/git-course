---
layout: page
title: Version control with Git  
subtitle: How, what and when to commit
---

> ## Learning objectives {.objectives}
> * Understand what makes a good commit message
> * Know which types of files not to commit
> * Know when to commit changes

### How to write a good commit message

Commit messages should explain why you have made your changes. They should mean
something to others who may read them - including your future self in 6 months
from now.

[Here is an excellent summary](http://chris.beams.io/posts/git-commit/) of
best-practice. It's well worth a read but the key points are given below:

1. Separate the subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how

---

### Commit anything that cannot be automatically recreated

Typically we use version control to save anything that we create manually
e.g. source code, scripts, notes, plain-text documents, LaTeX documents.
Anything that we create using a compiler or a tool e.g. object files (`.o`,
`.a`, `.class`, `.pdf`, `.dvi` etc), binaries (`exe` files), libraries (`dll`
or `jar` files) we don't save as we can recreate it from the source. Adopting
this approach also means there's no risk of the auto-generated files becoming
out of sync with the manual ones.

---

### When to commit changes?

There are no hard and fast rules, but good commits are atomic - they are the
smallest change that remain meaningful.  

In the same way that it is wise to frequently save a document that you are
working on, so too is it wise to save numerous revisions of your files. More
frequent commits increase the granularity of your "undo" button.

While DropBox and GoogleDrive also preserve every version, they delete old
versions after 30 days, or, for GoogleDrive, 100 revisions. DropBox allows for
old versions to be stored for longer but you have to pay for this. Using
revision control the only bound is how much space you have!

For code, it's useful to commit changes that can be reviewed by someone else 
in under an hour. 

----

> **What we know about software development - code reviews work** 
>
> Fagan (1976) discovered that a rigorous inspection can remove 60-90% of
errors before the first test is run. [Design and Code
inspections to reduce errors in program
development](http://www.mfagan.com/pdfs/ibmfagan.pdf). IBM Systems Journal 15
(3): pp. 182-211.
>
> **What we know about software development - code reviews should be about 60
minutes long** 
>
> Cohen (2006) discovered that all the value of a code review comes within the
first hour, after which reviewers can become exhausted and the issues they find
become ever more trivial.
[Best Kept Secrets of Peer Code
Review](http://smartbear.com/SmartBear/media/pdfs/best-kept-secrets-of-peer-code-review.pdf). SmartBear, 2006. ISBN-10: 1599160676. ISBN-13: 978-1599160672.

----

