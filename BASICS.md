# Git Basics

This workshop uses placeholders like `<USERNAME>`.
Fill them out when typing commands.

Teacher and student actions are indicated in bold.

* [Configuration](#configuration)
* [Cloning](#cloning)
* [Making and staging changes](#making-and-staging-changes)
* [Committing](#committing)
* [Rebasing](#rebasing)

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
git alias graph "log --pretty=oneline --graph --decorate --all"
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

# You have changed the "working directory". The change is untracked.
git status

# Show a diff of your untracked changes.
git diff

# Add the file.
git add <USERNAME>.txt

# You have updated the "index" with the content of the working directory.
# The file is now "staged" for the next commit.
git status

# Staged changes are not shown by the standard diff command.
git diff

# To see the changes staged for the next commit, use the --cached option.
git diff --cached
```

## Committing

**Each student in turn...**

```bash
# Update your local repo.
git pull

# Commit your staged changes. The -m option sets the commit message.
git commit -m "I am <USERNAME>."

# Push your changes to the remote repo.
git push

# The working directory and index are now clean.
git status
```

## Rebasing

**All students...**

```bash
# Update your local repo.
git pull

# See everyone's commits in the log.
git log

# Make a change in your file.
echo "Quizzical twins proved my hijack-bug fix." >> <USERNAME>.txt

# Add and commit it.
git commit -a -m "Updated <USERNAME>.txt"
```

**Teacher...**

```bash
# Clone the workshop repository.
git clone git@github.com:lotaris/<WORKSHOP>.git

# Create a text file named after your username.
echo "The quick brown fox jumps over the lazy dog." > <USERNAME>.txt

# Add and commit it.
git commit -a -m "I am <USERNAME>."

# Push your changes.
git push
```

**Each student in turn...**

```bash
# Try to push your change.
# This will fail because you are not up to date with the remote repo.
git push

# You can fetch the changes of the remote repo (without applying them).
git fetch

# Using the alias we defined at the beginning,
# the log will show you the changes in the remote repo.
git graph

# You have not pushed your commit yet, so you can still change it to follow the remote repo's history.
# It is called "rebasing" your commit. Your commit is re-applied after the existing history.
git rebase origin/master
```
