# Git Basics

This workshop uses placeholders like `<USERNAME>`.
Fill them out when typing commands.

Teacher and student actions are indicated in bold.

* [Configuration](#configuration)
* [Cloning](#cloning)
* [Making and staging changes](#making-and-staging-changes)
* [Committing](#committing)

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
echo 'The quick brown fox jumps over the lazy dog' > <USERNAME>.txt

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
git commit -m "My file."

# Push your changes to the remote repo.
git push

# The working directory and index are now clean.
git status
```
