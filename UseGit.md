# Using Git

Git has become the tool of choice
for managing history, versions,
and collaboration of anything
that is build from source code,
including LaTeX documents.
The web is full of Git tutorials,
so this tutorial will focus only
on things you need to get LaTeX work done.

A few other helpful web tutorials are:

[BitBucket's tutorial](https://www.atlassian.com/git/?utm_source=bitbucket&utm_medium=link&utm_campaign=help_dropdown&utm_content=learn_git)

## Creating a Local Repository

In order to convert any normal directory
in your file system into a Git repository,
one uses `git init`:

```
cd ~/papers/nature_2050/
git init
```

If you already have files in this repository,
such as the journal's format files or even
a first draft document, you should add them
and commit them:

```
git add journal.cls nature_2050.tex
git commit -m "journal class and first draft"
```

If you are writing this paper alone
and don't want an online backup, skip
straight to the section on making changes.
Otherwise, follow the instructions
for creating a host repository
and then connect your local
repository to the host using the host URL:

```
git remote add origin git@github.com:user/title.git
git push -u origin master
```

## Creating a Host Repository

There are numerous hosting options for the central
repository, and each of them has different mechanisms
for creating a new repository or paper.
Here are a few decent ones:

[BitBucket](https://bitbucket.org/)

[GitHub](https://github.com)

[Overleaf](https://www.overleaf.com)

[Authorea](https://www.authorea.com)

The result of all these services will be a Git URL
you can use in the next section, such as
`https://github.com/user/title.git` or
`git@github.com:user/title.git`.

## Copying an Existing Repository

The get your own copy of a host repository,
use `git clone` with the URL.
You can also provide an optional destination
where this repository will be created.

```
git clone git@github.com:princeton/einstein.git ~/papers/einstein/
```

If a Makefile has already been set up in this
repository, we can compile the paper and look at it:

```
cd ~/papers/einstein/
make
open einstein.pdf
```

There may be errors related to missing packages,
refer to the [compilation documentation](Compile.md) on this.

## Making Changes

As usual, you edit the relevant `.tex` file
to make changes to the paper, possibly also
editing the `.bib` file or adding figures.
To see the outcome, recompile to get a new PDF
document:

```
make
```

The first thing to be disciplined about is organizing your
changes into small "commit"s.
A good rule of thumb is that your changes should be
describable by a specific sentence, for example
"added performance figure" or "fixed grammar in Section 4"
as opposed to "all my changes from this week".
Git will require you to annotate your changes with
such a sentence.

After your current set of changes compile well,
you should check the general condition of things:

```
git status
```

This will show files which are changed and files
which are not tracked by Git.
Files you have changed or created should
be added with `git add`:

```
git add einstein.tex newfigure.eps
git status
```

`git status` will now show you what changes are
up for committing, and if you got everything
you can commit.

```
git commit -m "explain our statistical methods"
```

A convenient option is to add all changed
files before committing: `git commit -a -m`.
Note that this does not catch newly added files.

Note that a Git commit is a local action,
your repository is self-contained in its own
right and can accept commits in a laptop
at cruising altitude.

## Collaboration

Authors will collaborate through the host
repository by interactions between their
local repository and the host.
Before starting these interactions, ensure
all changes are committed.

The first collaborative step is to pull
the latest changes made by other authors from the host:

```
git pull
```

If they have not edited the same lines as
you, the merge will be successful.
Otherwise, Git will declare that you have
merge conflicts, and the files with
conflicts will have them annotated like so:

```
<<<<<<<<<<<<<<<<
this text as the others have it
================
this text as you have it
>>>>>>>>>>>>>>>>
```

If you fixed conflicts then add those
files and commit:

```
git add einstein.tex
git commit
```

Now your local copy has all the changes that
the host knows about.
To inform the host about your changes, use

```
git push
```

Note that your changes will only be accepted
if you have already pulled all the host's
changes, so that the host computer never
has to resolve serious conflicts.
