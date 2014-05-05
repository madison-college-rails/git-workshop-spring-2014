# Advanced Git

This self-study part of the workshop teaches advanced git operations.
For each subject, it is assumed that you start on the master branch
of your repository with a clean working tree and index.

* [Stashing](#stashing)
* [Interactive Add](#interactive-add)
* [Reverting Changes](#reverting-changes)



# Setup

Create a repository with your GitHub account and clone it.

```bash
git clone git@github.com:<YOUR_USERNAME>/<YOUR_REPO>.git
```



## Stashing

```bash
# Make staged and unstaged changes.
echo "The job of waxing linoleum frequently peeves chintzy kids." > stashing.txt
git add stashing.txt
echo "Back in June we delivered oxygen equipment of the same size." >> stashing.txt
git status

# Let's say you have to switch to another branch to work on something else.
# You have uncommitted changes which could conflict with that branch.
# "Stashing" saves your local modifications to a temporary dirty space and
# leaves you with a clean state.
git stash

# Your working tree and index are now clean.
git status

# You can switch branches.
git checkout -b other-branch

# Once you're done with your work, switch back to the original branch.
git checkout master

# Re-apply the changes you stashed earlier.
git stash apply

# You can stash multiple sets of changes and manage your stashes.
git help stash
```



## Interactive Add

```bash
# Download and commit the sample interactive.txt file into your repo.
curl -o interactive.txt https://raw.github.com/lotaris/git-workshop/master/interactive.txt
git add interactive.txt
git commit -m "Interactive add sample file."

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

# You can see that you have committed both sets of changes separately.
git log --patch
```



## Reverting Changes

```bash
# Commit a file.
echo "All questions asked by five watched experts amaze the judge." > reverting.txt
git add reverting.txt
git commit -m "Reverting sample file."

# Commit a change to this file.
echo "A quick movement of the enemy will jeopardize six gunboats." >> reverting.txt
git commit -a -m "Bad change."

# Push this change to the remote repo.
git push origin master

# The commit you have pushed is now part of the history so you can't
# remove it with a soft or hard reset. You have to create a new commit
# which cancels your change. You can also do this with the reset command.
git reset HEAD~1

# This has applied the state of the previous commit to the index.
git status
git diff --staged

# Note that it leaves the working tree untouched,
# so the changes of your last commit are still there.
git diff

# You can commit.
git commit -m "Reverted latest changes."

# Get rid of the reverted changes.
git reset --hard

# The contents of the file are now the same as before your second commit.
cat reverting.txt
```
