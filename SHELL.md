# Adding Git to your shell prompt

## Bash Users

Download the git-prompt script from the Git repository:

    curl https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh -o ~/.git-prompt.sh

Add the following to your `~/.bashrc` file:

```bash
source ~/.git-prompt.sh
export PS1="\t \u \w\$(__git_ps1) \$ "
```

Source: http://code-worrier.com/blog/git-branch-in-bash-prompt/
