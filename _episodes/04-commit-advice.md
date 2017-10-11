---
title: "Commit advice" 
teaching: 10
exercises: 0
questions:
- "How, what, and when to commit?"
- "What makes a good commit message?"
objectives:
- "Understand what makes a good commit message"
- "Know which types of files not to commit"
- "Know when to commit changes"
keypoints:
- "Commit messages explain why changes were made, so make them clear and concise"
- "Follow conventions to give a history that is both useful, and easy to read"
- "Only commit files which can't be automatically recreated"
---

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

We can automatically ignore such files using a `.gitignore` file.
See [hints and tips]({{page.root}}/12-hints-and-tips).

---

### When to commit changes?

- Commit frequently.
	- There are no hard and fast rules, but good commits are atomic -
	  they are the smallest change that remain meaningful.
	- In the same way that it is wise to frequently save a document that you are
	  working on, so too is it wise to save numerous revisions of your files.
	  More frequent commits increase the granularity of your "undo" button.
	- Small commits also help to avoid large merge conflicts.
- Test before you commit
	- Don't commit changes until you've tested that your code works.
	- Non-working code should be fixed before you commit.
- Don't commit unfinished work
	- Break your code changes into small, but working chunks.
	- If you need to temporarily save some work-in-progress
	  (e.g. in order to work in another branch),
	  use `git stash` -- see [hints and tips]({{page.root}}/12-hints-and-tips).
- Commit related changes.
	- Confine your commit to directly related changes.
	  If you fix two separate bugs, you should have two separate commits.
