# Advanced Git

This self-study part of the workshop teaches advanced git operations.
For each subject, it is assumed that you start on the master branch
of your repository with a clean working tree and index.

* [Stashing](#stashing)
* [Interactive Add](#interactive-add)



## Stashing

```bash
# If you have uncommitted changes on a branch and you need to switch
# to another branch, you can "stash" these changes into a temporary workspace.
git stash

# Your working tree and index are now clean and you can switch branches.
git checkout other-branch

# Once you're done with your work, switch back to the original branch.
git checkout original-branch

# Re-apply stashed changes.
git stash apply
```



## Interactive Add

Download [`interactive.txt`](interactive.txt) into your repo and commit it.

```bash
# Make one change in each paragraph of the file.
vim interactive.txt

# Let's say some of these changes are related to one feature,
# while others are related to a bugfix, and you'd like to commit
# them separately.
# You can interactively add parts of your changes with the --patch option.
git add --patch interactive.txt

# Git will show you a diff of each change which you can accept with `y` or reject with `n`.
# If the selected change is too big and you would like to split it, type `s` and it will
# ask you again with smaller parts of the change.
# Make the first commit when you're done selecting your changes.
git commit -m "Feature."

# Then you can add the rest of the changes and commit the bugfix.
git commit -a -m "Bugfix."
```
