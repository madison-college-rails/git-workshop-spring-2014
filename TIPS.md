# Tips & Tricks

* [Aliases](#aliases)
* [Log](#log)



## Aliases

```bash
# Git has no aliases by default.
# If you want to commit with `git ci` instead of `git commit`, for example,
# you can use the alias command.
git alias ci commit

# You can define more complicated aliases with options.
git alias graph "log --oneline --graph --decorate"

# Aliases are added to your ~/.gitconfig file.
# You can also modify this file directly.
cat ~/.gitconfig
```

* [Pro Git - Customizing Git](http://git-scm.com/book/ch7-1.html)
* [Git Config Reference](https://www.kernel.org/pub/software/scm/git/docs/git-config.html)
* [Sample Git Config](https://github.com/AlphaHydrae/env/blob/master/.gitconfig)



## Log

```bash
# The log command has a plethora of options to select
# which commits to log and to customize its output.
# For example, this shows the log with full diffs:
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
