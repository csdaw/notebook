


# Git and GitHub

## Branches

Delete local branch.


```bash
git branch -d nameofbranch
```

See list of available remotes.


```bash
git remote -v
```

Clean up old local branches that are no longer on the remote called origin.


```bash
git remote prune origin
```

## Commits

Remove last n local commits, where n >= 1.


```bash
git reset HEAD~n
```


## GitHub actions

If your tests have failed, you can download and inspect the artifacts produced
during the workflow by navigating to:

1. Actions
1. Click on the workflow run of interest
1. Scroll down to 'Artifacts'
1. Click on the bundle of artifacts you want to download

## Submodules

Cloning a repository that has submodules:


```bash
git clone git@github.com:csdaw/repo.git
cd repo
git submodule update --init --recursive
```

If you want to make changes to the submodule and commit and push as usual, make
sure you checkout a branch, like so:


```bash
git checkout main
```
