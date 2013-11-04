# Git Basics

This workshop uses placeholders like `<USERNAME>`.
Fill them out when typing commands.

Teacher and student actions are indicated in bold.

* [Configuration](#configuration)
* [Cloning](#cloning)
* [Making and staging changes](#making-and-staging-changes)
* [Remotes](#remotes)
* [Committing](#committing)
* [Rebasing](#rebasing)
* [Branching](#branching)
* [Merging](#merging)
* [Conflict Resolution](#conflict-resolution)
* [Amending Commits](#amending-commits)
* [Resetting Changes](#resetting-changes)



## Setup

**Teacher...**

Create a repo named `<WORKSHOP>`, give all students push access.



## Configuration

**All students...**

```bash
# Set up your user name and e-mail address.
# This information will be immutably included into every commit you make.
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# Set up this alias for the workshop.
git config --global alias.graph "log --pretty=oneline --graph --decorate --all"

# You can see all your configuration in the following file.
cat ~/.gitconfig
```



## Cloning

**All students...**

```bash
# Get a copy of the workshop repository.
git clone git@github.com:lotaris/<WORKSHOP>.git
```



## Making and staging changes

**All students...**

```bash
# Check the status of the repo.
git status

# Create a text file named after your username.
echo "The quick brown fox jumps over the lazy dog." > <USERNAME>.txt

# You have changed the "working tree". The change is untracked.
git status

# Show a diff of your untracked changes.
git diff

# Add the file.
git add <USERNAME>.txt

# You have updated the "index" with the content of the working tree.
# The file is now "staged" for the next commit.
git status

# Staged changes are not shown by the standard diff command.
git diff

# To see the changes staged for the next commit, use the --staged option.
git diff --staged

# Make a change in your file.
echo "Fix problem quickly with galvanized jets." >> <USERNAME>.txt

# Your new changes are untracked.
git status
git diff

# The index still contains the first version of the file you staged earlier.
git diff --staged

# To include the new file in the next commit, you must stage it.
git add <USERNAME>.txt
git status
git diff --staged
```



## Remotes

**All students...**

```bash
# "Remotes" are other repositories that you can synchronize with.
# The "origin" remote was automatically added when you cloned.
git remote

# You can see the URLs of remotes with the verbose option.
git remote -v
```



## Committing

**Each student in turn...**

```bash
# Update your local repo.
git pull origin master

# Commit your staged changes. The -m option sets the commit message.
git commit -m "I am <USERNAME>."

# Push your changes to the remote repo.
git push origin master

# The working directory and index are now clean.
git status
```



## Rebasing

**All students...**

```bash
# Update your local repo.
git pull origin master

# See everyone's commits in the log.
git log

# Make a change to your file.
echo "Quizzical twins proved my hijack-bug fix." >> <USERNAME>.txt

# Add and commit it.
git add <USERNAME>.txt
git commit -m "Updated <USERNAME>"
```

**Teacher...**

```bash
# Clone the workshop repository.
git clone git@github.com:lotaris/<WORKSHOP>.git

# Create a text file named after your username.
echo "Five jumping wizards hex bolty quick." > <USERNAME>.txt

# Add and commit it.
git add <USERNAME>.txt
git commit -m "I am <USERNAME>."

# Push your changes.
git push origin master
```

**Each student in turn...**

```bash
# Try to push your change.
# This will fail because you are not up to date with the remote repo.
git push origin master

# You can fetch the changes of the remote repo (without applying them).
git fetch origin

# Using the alias we defined at the beginning,
# the log will show you the changes in the remote repo.
git graph

# You have not pushed your commit yet, so you can still change it to follow the remote repo's history.
# It is called "rebasing" your commit. Your commit is re-applied after the existing history.
git rebase origin/master

# Your commit now follows the remote repo's history.
git graph

# And you can push it.
git push origin master
```



## Branching

You are going to work in a separate branch and push that work to the remote repository.
Once that's done, you will merge your changes back into the master branch.

**All students...**

```bash
# Use the branch command to list branches and see which one you are on.
# By convention, the default branch is the "master" branch.
git branch

# Create a new branch.
git branch <USERNAME>-branch

# Switch to your branch.
git checkout <USERNAME>-branch

# Check that you are on the right branch.
git branch

# Make a change to your file and commit it.
echo "Twelve ziggurats quickly jumped a finch box." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "Updated <USERNAME> again."

# You can see in the log that your branch has diverged from master.
git graph

# Switch between branches with the checkout command.
# This updates the working directory with the state of that branch.
git checkout master

# Go back to your branch.
git checkout <USERNAME>-branch
```

**Teacher...**

```bash
# Commit a change to your file.
echo "Five quacking zephyrs jolt my wax bed." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "Updated my file."
```

**Each student in turn...**

```bash
# You're now done working in the branch.
# You want to merge these changes back into the master branch.
# You must make sure that your master branch is up to date first.
# Switch to the master branch and update it from the remote repo.
git checkout master
git pull origin master

# Switch back to your branch and rebase it on the master branch.
git checkout <USERNAME>-branch
git rebase master

# The commit in your branch should now follow the last commit from the master branch.
git graph

# Go back to the master branch.
git checkout master

# Merge your branch into master.
git merge <USERNAME>-branch

# Your commit is now also on the master branch.
git graph

# Push the updated master branch to the remote repo.
git push origin master
```

**All students...**

```bash
# Now that your branch has been merged into master, you can delete it.
git branch -d <USERNAME>-branch
```

**All students...**

```bash
# Update your local repo.
git fetch origin

# You can see that all user branches have been deleted.
git branch -a
```



## Merging

**Teacher..**

```bash
# Create a branch for everyone to work in.
git checkout master
git branch feature-shared
git push origin feature-shared
```

**All students...**

```bash
# Fetch all branches from the remote repo.
git fetch origin

# You can see all the branches that were pushed.
git branch -a

# Switch to the shared branch.
git checkout feature-shared
```

**Each student in turn...**

```bash
# Make sure your local copy of the branch is up to date.
git pull origin feature-shared

# Make a change to your file and commit it.
echo "My ex pub quiz crowd gave joyful thanks." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "I changed it. Duh."

# Push your changes to the remote repo.
git push origin feature-shared
```

**Teacher (before each student)...**

```bash
# Commit a change to your file on master and push it.
git checkout master
git pull origin master
echo "Cozy sphinx waves quart jug of bad milk." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "More pangrams."
git push origin master
```

**Each student in turn...**

```bash
# You still have to work on the feature-shared branch, but it has
# now diverged from the master branch, and you want to keep it up to date.
# You can't rebase the commits of the branch because they have been
# pushed; that commit history is already shared with other people.

# First, make sure your master branch is up to date.
git checkout master
git pull origin master

# Also make sure you have all the commits in the branch.
git checkout feature-shared
git pull origin feature-shared

# You can now merge master into the branch.
git merge master

# This has created a new commit.
git graph

# You can push this commit to the remote repo.
git push origin feature-shared
```



## Conflict Resolution

**Teacher...**

```bash
# In the master branch, make a change to the first line of each student's file and push it.
git checkout master
git pull origin master
vim <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "Fixing things."
git push origin master
```

**All students...**

```bash
# Switch back to the master branch
git checkout master

# In your file, add a word at the end of the first line. Save and commit your change.
vim <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "Fixing things."

# Try to push it.
# This will not work because the master branch has changed since then.
git push origin master
```

**Each student in turn...**

```bash
# You want to get the latest changes of the master branch and rebase your commit.
git pull --rebase

# Oops. You're now in a conflicted state because your file was changed by someone else.
# Find the conflict markers in your file and resolve the conflict.
vim <USERNAME>.txt

# Once you're done, add the file to mark the conflict as resolved.
git add <USERNAME>.txt

# Continue rebasing.
git rebase --continue

# Push your change.
git push origin master
```



## Amending Commits

**All students...**

```bash
# Commit a change with a mistake in your file.
echo "Blowzy red vixens fight for a quick jummp." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "I can't spell."

# You haven't push this commit yet, so you can still fix it.
# Fix the mistake and stage the file.
vim <USERNAME>.txt
git add <USERNAME>.txt

# Then "amend" the last commit.
# Your commit is now fixed, ready to be pushed.
git commit --amend -m "More texxt."

# If you made a mistake in the commit message, you can also fix just that.
git commit --amend -m "More text."
```



## Resetting Changes

**All students...**

```bash
# Make a change and stage it.
echo "When zombies arrive, quickly fax judge Pat." >> <USERNAME>.txt
git add <USERNAME>.txt

# Make another change.
echo "Waxy and quivering, jocks fumble the pizza." >> <USERNAME>.txt

# You now have staged and untracked changes.
git status

# If you realize that what you've been doing since the last commit
# is completely wrong and you just want to get rid of it, you can
# reset the working tree to the last commit.
# This operation is destructive. There is no way to cancel it.
git reset --hard

# Commit a change this time.
echo "A quick chop jolted my big sexy frozen wives." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "More text."

# Let's say you know something is missing from this commit, but you're
# not yet sure what, so you would like to cancel it and re-commit later.
# You can do a soft reset to the previous commit.
git reset --soft HEAD~1

# This leaves the working tree and index as they are, but moves HEAD to
# the previous commit. Your commit doesn't exist anymore.
# NEVER EVER do this with a commit that you have already pushed.
git status

# Commit it again.
git commit -m "More text."

# If you have pushed it, it is now part of the history so you can't
# remove it. You have to create a new commit which cancels your change.
# You can also do this with the reset command.
git reset HEAD~1

# This will apply the state of the previous commit to the working tree and index.
# You can commit this and push it to the remote repo to cancel your change.
git status
git diff --staged
```
