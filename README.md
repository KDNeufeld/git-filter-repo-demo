# git-filter-repo-demo

This repo demonstrates git-filter-repo for rewriting history.

## git-filter-repo basics

git-filter-repo is a tool for rewriting git history.

### Tip

Always backup before rewriting history!

### Warning

Rewriting history can be destructive!

## How to Replace a Line in a Past Commit Using git-filter-repo

GitHub has a very nice how-to document that details this process in full
(https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository), 
and a git-filter-repo man page (https://htmlpreview.github.io/?https://github.com/newren/git-filter-repo/blob/docs/html/git-filter-repo.html), 
but for brevity,

Suppose you want to replace a line in `sample.txt` that was added in an old commit.

1. Install git-filter-repo:
```pip install git-filter-repo```

2. Run git-filter-repo with a file-replacement script:

First, create a text file called replacement-text.txt with your replacement rules. There should be one line per expression to scan for, ie:
```
p455w0rd
foo==>bar
glob:*666*==>
regex:\bdriver\b==>pilot
literal:MM/DD/YYYY==>YYYY-MM-DD
regex:([0-9]{2})/([0-9]{2})/([0-9]{4})==>\3-\1-\2
```

And then run:

```git filter-repo --replace-text expressions.txt C:\Temp\replacement-text.txt --sensitive-data-removal --force```

will go through and replace `p455w0rd` with `***REMOVED***`, `foo` with `bar`, any line containing `666` with a blank line, the word `driver` with `pilot` (but not if it has letters before or after; e.g. drivers will be unmodified), replace the exact text `MM/DD/YYYY` with `YYYY-MM-DD` and replace date strings of the form MM/DD/YYYY with ones of the form YYYY-MM-DD. In the expressions file, there are a few things to note:

Every line has a replacement, given by whatever is on the right of ==>. If ==> does not appear on the line, the default replacement is `***REMOVED***`.

Lines can start with literal:, glob:, or regex: to specify whether to do literal string matches, globs (see https://docs.python.org/3/library/fnmatch.html), or regular expressions (see https://docs.python.org/3/library/re.html#regular-expression-syntax). If none of these are specified, literal: is assumed.

If multiple matches are found, all are replaced.

*Note: do not use the --path or --paths-from-file params as these will remove all files except those being changed.
*Note: this command needs to be run on a clean checkout and GitKraken needs to be closed or the tool will fail.
*Note: there is currently no known way to only scan certain files in the repository

- This replaces all occurrences of "Old line to be replaced." with "New replacement line." in `sample.txt` across history.

3. Review rewritten history and force-push once you're sure and happy with your local version:

```git push --force --mirror origin```

**Note:** Always backup your repo before rewriting history.

