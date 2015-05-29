# Compiling a LaTeX Document

LaTeX documents are generated
from a set of input files,
most importantly a set of
files ending in `.tex`.
For the majority of this
tutorial we will assume
your document resides mainly in
one `.tex` file.
A journal paper typically
uses BibTeX for references, which
add a `.bib` file, and figures.
Finally, journals will provide
formatting files specifying
how articles and bibliographies
should look in their journal.

Read the section on Makefiles below
first, and refer to the other sections
for details when questions arise.

## Makefile

One might think that we should provide
a robust general-purpose Makefile
for LaTeX documents.
It turns out that this is quite hard,
and so instead we will provide details
of how the compilers work and some
guidance for example Makefiles.

People familiar with `make` might be inclined
to encode dependencies such that
each compiler command is run only when it
absolutely must be.
The main reason this is hard is that
`make` was not designed to deal with
files that go through several transformations,
for example the `.aux` file.

Because the compilers are reasonably fast
even for the majority of journal papers,
one suggestion might be to encode the entire
process as one `make` rule that takes all
source files as input and results in the document:

```
NAME=einstein_1916

# you can add other source files to the end of this line:
$(NAME).pdf: $(NAME).tex $(NAME).bib
	pdflatex $(NAME)
	bibtex $(NAME)
	pdflatex $(NAME)
	pdflatex $(NAME)

clean:
	rm -rf *.log *.aux *.bbl *.blg $(NAME).pdf
```

For `latex`, it could be something like this:

```
NAME=einstein_1916

# you can add other source files to the end of this line:
$(NAME).pdf: $(NAME).tex $(NAME).bib fig1.eps
	latex $(NAME)
	bibtex $(NAME)
	latex $(NAME)
	latex $(NAME)
        dvips $(NAME).tex -o $(NAME).ps
        ps2pdf $(NAME).ps $(NAME).pdf

clean:
	rm -rf *.log *.aux *.bbl *.blg $(NAME).pdf
```

Folks who want to make more fine-grained,
complex Makefiles may begin by studying
the[section on files](## File Types).

## latex v.s. pdflatex

There are two programs commonly used
to generate these documents,
`latex` and `pdflatex`.
`pdflatex` directly generates a PDF
file, which is usually what we're
aiming for.
`latex` generates a `.dvi` file, which
can then be converted to a PDF file in various ways.

There is `dvips`, which converts to PostScript format,
and `ps2pdf` which converts PostScript to PDF.
Also there is `dvipdfm` which converts directly
from DVI to PDF.

One reason we would use `latex` is some
styles dictated by journals will compile
better with `latex` as opposed to `pdflatex`.
Both of these programs also output
other files described [below](## File Types).

`pdflatex` also accepts more image formats
for figures, whereas `latex` only really acceps
`.eps` format figures.
Ironically, `pdflatex` doesn't accept `.eps`
figures every well, so image formats may
guide your choice of which to use.

## cross-references

Lets assume your document source is
in `amazing.tex`.
If you don't have any cross references
(uses of `\label` and `\ref`), then your
document will compile with a single command:

```
pdflatex amazing
```

If you do have cross references, you need
to run the compiler over the file twice.
The first pass will associate `\label`
names with actual numbers like Section 2.4,
and store that in a `.aux` file.
The second pass will replace every `\ref` command
with the right number from the `.aux` file.

```
pdflatex amazing
pdflatex amazing
```

There are also unconfirmed rumors of having
to repeat this more than twice.

## BibTeX

When using BibTeX, you have something like:

```
\bibliographystyle{plain}
\bibliography{works}
```

and a file named `works.bib`, replace `works` with
any name you like.
You will need to run the compilers even more.
First, an initial run to generate the `.aux` file,
which BibTeX needs.
Then actually run BibTeX, which will use the `.aux`
file and the `.bib` and `.bst` files to create
a `.bbl` file.
After that you run the LaTeX compiler as before,
as if your bibliography were done with
plain `\bibitem` lines:

```
pdflatex amazing
bibtex amazing
pdflatex amazing
pdflatex amazing
```

## File Types

[Here](http://en.wikibooks.org/wiki/LaTeX/Basics#Ancillary_files)
is a really good guide to all the various files
output by the LaTeX compiler and what their purpose is.
So good, in fact, that we will replicate some of the
information here in two tables, for extensions that
are commonly seen.

First, the source files that actually affect the result,
these files should be tracked by Git and not deleted:

| Extension | Description                          |
|-----------|--------------------------------------|
|`.tex`     | LaTeX or TeX input file. It can be compiled with latex. |
|`.bib`     | Bibliography database file. (where you can store a list of full bibliographic citations) |
|`.cls`     | `\documentclass` files define what your document looks like, often given by journal. |
|`.sty`     | `\usepackage` LaTeX Macro package. |
|`.bst`     | `\bibliographystyle` BiBTeX style file. |

Second, the files generated by running the various tools:

| Extension | Description                          |
|-----------|--------------------------------------|
|`.aux`     | Transports information from one compiler run to the next, including cross-references. |
|`.bbl`     | Bibliography file output by BibTeX and used by LaTeX (contains generated `\bibitem` lines) |
|`.blg`     | BibTeX log file. (BibTeX errors are logged here) |
|`.dvi`     | Device Independent File resulting from `latex` |
|`.pdf`     | Portable Document Format resulting from `pdflatex` or `dvipdfm` |
|`.log`     | Gives a detailed account of what happened during the last `latex` run |
|`.out`     | hyperref package file, contains URL and cross-reference info |
|`.toc`     | Stores all section headers. Used by next `latex` run to produce the table of contents. |
