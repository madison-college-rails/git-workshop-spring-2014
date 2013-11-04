# Tips & Tricks

* [Aliases](#aliases)
* [Log](#log)
* [Stashing](#stashing)
* [Interactive Add](#interactive-add)
* [Additional Tools](#additional-tools)



## Aliases

```bash
# Git has no aliases by default.
# If you want to commit with `git ci` instead of `git commit`, for example,
# you can use the alias command.
git config --global alias.ci commit

# You can define more complicated aliases with options.
git config --global alias.graph "log --oneline --graph --decorate"

# Aliases are added to your ~/.gitconfig file.
# You can also modify this file directly.
cat ~/.gitconfig
```

* [Pro Git - Customizing Git](http://git-scm.com/book/ch7-1.html)
* [Git Config Reference](https://www.kernel.org/pub/software/scm/git/docs/git-config.html)
* [Sample Git Config](https://github.com/AlphaHydrae/env/blob/master/.gitconfig)



## Log

The log command has a plethora of options to select which commits to log and to customize its output.

```bash
# This shows the log with full diffs:
git log --patch

# This shows the log with one line per commit, which is more readable.
git log --pretty=oneline

# The --decorate option adds branch markers.
git log --pretty=oneline --decorate

# The --graph option shows the history of commits as an ASCII graph.
git log --pretty=oneline --graph

# The --all option shows commits from all branches, not just from the history of the current branch.
git log --pretty=oneline --decorate --all

# This shows a graph of commits on all branches with branch markers.
git log --pretty=oneline --graph --decorate --all
```



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



## Additional Tools

[git-extras](https://github.com/visionmedia/git-extras) is a package of additional git commands.
It provides useful aliases like `alias` or `ignore`.
It also provides commands to analyze a repo like `summary` which lists authors and the number of contributions they made.

[hub](http://hub.github.com) is a GitHub wrapper for git.
Where previously you would write `git clone https://github.com/lotaris/git-workshop.git`, you can just write `git clone lotaris/git-workshop`.
It also provides useful commands for forks and pull requests.
