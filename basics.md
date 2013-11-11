# Git Basics

This part of the workshop teaches common git operations like committing files, using branches, rebasing, merging and resolving conflicts.
It also highlights some of the differences of git from subversion.

It is recommended to at least skim through the following chapters of the
[Pro Git](http://git-scm.com/book) book before the workshop: 1.1, 1.3, 2.\*, 3.1, 3.2.

The workshop is meant for one teacher and several students.
It takes about two hours and a half to complete with five students.
What the teacher and students must do is indicated in bold.
Instructions preceded by *Each student in turn...* must be executed one student at a time to avoid conflicts
(or cause them when it is the purpose of the exercise).

Placeholders like `<USERNAME>` must be filled out when typing commands.

* [Configuration](#configuration)
* [Cloning](#cloning)
* [Remotes](#remotes)
* [Making and staging changes](#making-and-staging-changes)
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

```bash
# Clone the workshop repository.
git clone git@github.com:lotaris/<WORKSHOP>.git

# Commit a file named after your username.
echo "Few black taxis drive up major roads on quiet hazy nights." > <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "Workshop setup."

# Push your changes.
git push
```



## Configuration

**All students...**

```bash
# Set up your name and e-mail address.
# This information will be immutably included into every commit you make.
# If you want different user info for a repository, omit the --global option.
git config --global user.name "John Doe" # This should be your full name.
git config --global user.email johndoe@example.com

# Set up this alias for the workshop.
git config --global alias.graph "log --pretty=oneline --graph --decorate --all"

# You can see all your configuration in the following file.
cat ~/.gitconfig
```



## Remotes

**All students...**

```bash
# "Remotes" are other repositories that you can synchronize with.
# The "origin" remote was automatically added when you cloned.
# It is the remote repo you cloned from.
git remote

# You can see the URLs of remotes with the verbose option.
git remote -v

# You could have defined the origin remote yourself like this.
git remote add origin git@github.com:lotaris/<WORKSHOP>.git
```

*Note to SVN users:* the notion of centralized server doesn't exist in git.
If an organization decides to use a centralized server,
that is a convention; it is not enforced by git.
Any remote repo is indistinguishable from your own at the git level.



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

# Add the file.
git add <USERNAME>.txt

# You have updated the "index" with the contents of your file in the working tree.
# The file is now "staged" for the next commit.
git status

# Staged changes are not shown by the standard diff command.
git diff

# To see the changes staged for the next commit, use the --staged option.
git diff --staged

# Make a change to your file.
echo "Fix problem quickly with galvanized jets." >> <USERNAME>.txt

# Your new changes are untracked.
git status

# You can see them with the diff command.
git diff

# The index still contains the first version of the file you staged earlier.
git diff --staged

# To include the new file in the next commit, you must stage it.
git add <USERNAME>.txt
git status
git diff --staged
```

*Note to SVN users:*
the difference between the working tree and the index doesn't exist in SVN.
Keep in mind that you must always stage your changes for them to be committed,
even for files that are already tracked.
Before committing, you should always use the `status` command
so that you are sure of what is untracked and what is staged.



## Committing

**Each student in turn...**

```bash
# You will work with branches later in this workshop,
# but notice that you are currently on the "master" branch (by default).
git branch

# Before committing, make sure your local repo is up to date.
# In git, you do this by "pulling" changes from the remote repo.
# The remote we want to pull from is "origin", and we want
# to get the commits of the "master" branch.
git pull origin master

# Commit the changes you staged earlier.
# The -m option sets the commit message.
git commit -m "I am <USERNAME>."

# Now you want to share your commit with others,
# so you "push" your changes to the remote repo.
git push origin master

# The working directory and index are now clean.
git status
```

**All students...**

```bash
# Pull the latest commits.
# This will apply everyone's changes.
git pull origin master

# You can see everyone's commits in the log.
# Note that commits are identified by a hash.
# The hash is a SHA-1 hash based on the contents,
# author, date and parent of the commit.
git log

# Check the commit history graph (with the alias you defined earlier).
# You cannot really see the graph yet because there are few commits
# in a straight line, but it does show you the state of branches.
# Remember than in git branches are just pointers to a commit.
# Since you just pulled the latest changes, your local master branch
# points to the most recent commit.
# You can also see HEAD which points to the current commit. That's the
# same as the commit that master points to, since you are on the master branch.
git graph

# For some, the origin/master branch will be a few commits behind the
# master branch. That's because git shows you the last known state of
# the origin remote, which might have changed since.
# To get an up-to-date picture, you must "fetch" all changes from the remote.
# Fetch will get all commits without applying them.
git fetch origin

# Now origin/master and master should point to the same commit.
git graph
```

*Note to SVN users:* each student committed one after the other to avoid problems.
In SVN, these commits could have been made at any time since they affect different files.
Not so with git: commits must follow the previous commit history even when making changes to different files.



## Rebasing

For a remote repo to accept the commits that you push, those commits must follow the existing commit history.
However, sometimes your local commits will be out of date with that history.
For example, someone might have committed and pushed other changes while you were working.
Your commits are still based on an older commit.

If you have not pushed your commits yet, you have the option of *rebasing*.
Rebasing is the action of re-applying the changes of your commit(s) after other commits.
This will create new commits with different parents in the commit history.

**All students...**

```bash
# Make a change to your file.
echo "Quizzical twins proved my hijack-bug fix." >> <USERNAME>.txt

# Add and commit it.
git add <USERNAME>.txt
git commit -m "Updated <USERNAME>"
```

**Teacher...**

```bash
# Commit a change to your file.
git pull origin master
echo "Five jumping wizards hex bolty quick." >> <USERNAME>.txt
git commit -a -m "I am <USERNAME>."

# Push your changes.
git push origin master
```

**Each student in turn...**

```bash
# Try to push your change.
# This will fail because you are not up to date with the remote repo.
git push origin master

# Fetch the changes from the remote repo.
git fetch origin

# The commit graph will show you what has changed in the remote repo.
# Your commit and the latest commit(s) on the remote have diverged.
git graph

# You have not pushed your commit yet, so you can still move it to follow
# the remote repo's history. It is called "rebasing" your commit.
# You are re-applying it on top of the master branch of the origin remote.
git rebase origin/master

# The commit now follows the remote repo's history.
# Notice that the commit hash has changed. The contents are the same,
# but it is a new commit since it has a different parent.
git graph

# And you can push it.
git push origin master
```

*Note:* check [rebasing in the Pro Git book](http://git-scm.com/book/en/Git-Branching-Rebasing)
for diagrams that explain rebasing.



## Branching

**All students...**

```bash
# Pull the latest changes.
git pull origin master

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

*Note to SVN users:* notice that branch creation is instantaneous in git.
That's because a branch is just a pointer to a commit.
Switching between branches is also fast because only the differences have to be applied to the working tree.

**Teacher...**

```bash
# Commit a change to your file.
git pull origin master
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

# You can see that your local branch has been deleted.
git branch
```



## Merging

**Teacher..**

```bash
# Create a branch for everyone to work in.
git checkout master
git pull origin master
git branch feature-shared
git push origin feature-shared
```

**All students...**

```bash
# Fetch all branches from the remote repo.
git fetch origin

# You can see all the branches that are on the remote.
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
git merge -m "Merged master into feature-shared." master

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
# Switch back to the master branch. DO NOT pull the latest changes.
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
# Usually you want to pull the latest changes and then rebase your own.
# The --rebase option of the pull command does just that.
# Get the latest changes of the master branch with a rebase.
git pull --rebase origin master

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

When you notice a mistake in your last commit, you can *amend* it before pushing it to a remote.
You can modify the contents, commit message, author, or any other commit information.
Keep in mind that amending works only for the last unpushed commit.

**All students...**

```bash
# Make sure you are up to date.
git pull origin master

# Commit a change with a mistake in your file.
echo "Blowzy red vixens fight for a quick jummp." >> <USERNAME>.txt
git add <USERNAME>.txt
git commit -m "I can't spell."

# You haven't push this commit yet, so you can still fix it.
# Fix the mistake and stage the file.
vim <USERNAME>.txt
git add <USERNAME>.txt

# Then "amend" the last commit.
# Instead of creating a new commit, this changes the last one.
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

# Your changes are no longer there.
git status
```

**All students...**

```bash
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
```

Soft and hard resets can modify the commit history,
so they should only be used for uncommitted or unpushed changes.
To revert commits that have already been pushed, see [reverting changes (advanced)](advanced.md#reverting-changes).
