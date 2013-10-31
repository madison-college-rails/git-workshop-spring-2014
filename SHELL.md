# Adding Git to your shell prompt

## Bash Users

Download the git-prompt script from the Git repository:

    curl https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh -o ~/.git-prompt.sh
    curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.sh

Add the following to your `~/.bashrc` file (or if you already have a `PS1` customization, add it there):

```bash
source ~/.git-prompt.sh
source ~/.git-completion.sh
export PS1="\t \u \w\$(__git_ps1) \$ "
```

Source: http://code-worrier.com/blog/git-branch-in-bash-prompt/
