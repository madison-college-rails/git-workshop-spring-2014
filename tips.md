# Tips & Tricks

* [Coloring](#coloring)
* [Aliases](#aliases)
* [Log](#log)
* [Additional Tools](#additional-tools)



## Coloring

Git can color some of its output to help you visualize things more easily.
You can use the following command to turn on automatic terminal coloring.

```bash
git config --global color.ui true
```



## Aliases

```bash
# Git has no aliases by default.
# If you want to commit with `git ci` instead of `git commit`, for example,
# you can use the alias command.
git config --global alias.ci commit

# You can define more complicated aliases with options.
git config --global alias.graph "log --oneline --graph --decorate --all"

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

# This logs one commit per line with the author name and a short date.
git log --pretty=format:"%h %an %ad" --date=short

# This shows an ASCII graph of commits on all branches with branch markers.
git log --pretty=oneline --graph --decorate --all

# This logs the commits between the master and the feature branch
# (the commits that are in feature but not in master).
git log master..feature

# This logs the commits between the master branch on origin and the local master branch.
# In other words, it shows all the commits on your local master branch that have not yet
# been pushed to the master branch of the origin remote.
git log origin/master..master
```



## Additional Tools

[git-extras](https://github.com/visionmedia/git-extras) is a package of additional git commands.
It provides useful aliases like `alias` or `ignore`.
It also provides commands to analyze a repo like `summary` which lists authors and the number of contributions they made.

[hub](http://hub.github.com) is a GitHub wrapper for git.
Where previously you would write `git clone https://github.com/lotaris/git-workshop.git`, you can just write `git clone lotaris/git-workshop`.
It also provides useful commands for forks and pull requests.
