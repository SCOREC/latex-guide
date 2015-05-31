# Tracking Changes

## Latexdiff

`latexdiff` is a program which takes as input
two `.tex` files and outputs a `.tex` file
which highlights the changes between them.
`latexdiff` is part of a LaTeX distribution,
and the relevant package manager
should be able to install it (see [Installing LaTeX](InstallLatex.md)).

```
latexdiff old.tex new.tex > diff.tex
```

To obtain old and new versions of your document,
query the versions stored in Git.
For example, to generate a document showing
your changes to `mypaper.tex` that have not
yet been pushed to the host, do the following:

```
git pull
git checkout origin/master mypaper.tex
cp mypaper.tex old.tex
git checkout master mypaper.tex
cp mypaper.tex new.tex
latexdiff old.tex new.tex > diff.tex
cp diff.tex mypaper.tex
make
cp mypaper.pdf diff.pdf
open diff.pdf
git checkout mypaper.tex
```

You can replace `origin/master` and `master`
above with any two Git version identifiers
to compare changes between those versions.
