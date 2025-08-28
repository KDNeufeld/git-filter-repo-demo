# git-filter-repo-demo

This repo demonstrates git-filter-repo for rewriting history.

## git-filter-repo basics

git-filter-repo is a tool for rewriting git history.

### Tip

Always backup before rewriting history!

### Warning

Rewriting history can be destructive!

## How to Replace a Line in a Past Commit Using git-filter-repo

Suppose you want to replace a line in `sample.txt` that was added in an old commit.

1. Install git-filter-repo:
```pip install git-filter-repo```

2. Run git-filter-repo with a file-replacement script:

First, create a text file called replacement-text.txt with your replacement rule. There should be one line per replacement to scan for:
```echo Old line to be replaced.==>New replacement line. > C:\Temp\replacement-text.txt```

```git filter-repo --replace-text C:\Temp\replacement-text.txt --sensitive-data-removal --force```

*Note: do not use the --path or --paths-from-file params as these will remove all files except those being changed.
*Note: this command needs to be run on a clean checkout and GitKraken needs to be closed or the tool will fail.
*Note: there is currently no known way to old scan certain files in the repository

- This replaces all occurrences of "Old line to be replaced." with "New replacement line." in `sample.txt` across history.

3. Review rewritten history and force-push if needed:

```git log --oneline --graph git push --force```

**Note:** Always backup your repo before rewriting history.

