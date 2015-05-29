# Install LaTeX

Installing LaTeX varies depending on your
Operating System, but we recommend installing
it such that you can use command line tools,
which is easiest on Linux or Mac.

[This](http://latex-project.org/ftp.html)
page from the LaTeX project itself describes
installations for the various operating systems.
Below are additional details from SCOREC experience.

TeX Live is a distribution containing
LaTeX and whole system for managing the
many packages of additional functionality available.
In fact, it has its own package manager,
the TeX Live Manager, or `tlmgr` on the command line.
The full collection of LaTeX packages
can be found in the Comprehensive TeX Archive Network,
or [CTAN](https://www.ctan.org/).
Frequently, when dealing with a new document
style or wanting additional features, one needs
to install missing packages.
With the TeX Live Manager, this can be as easy as

```
tlmgr install <package name>
```

CTAN is a good place to look when you simply
get an error talking about a missing filename
and you want to know which package will
bring it in.

## Linux

Your package manager almost certainly has a LaTeX
package, probably called texlive-something.
Subsequently, missing LaTeX packages are also usually available
bundled into OS packages.
So far, using the OS package manager is recommended,
no reports of using `tlmgr` on Linux yet.
For popular distros, doing a Google search for the
name of the file you are missing will typically
give some results pointing you to the right Linux package name.

## Mac

MacTeX is the recommended way, but it is large.
Success has been found by first installing
BasicTeX from the bottom of [this page](http://www.tug.org/mactex/morepackages.html).
The TeX Live Manager `tlmgr` can then be subsequently
used to install any missing packages.

## Windows

We currently don't have reports of successful
command line LaTeX installs on Windows,
if someone accomplishes this please update
this document.
