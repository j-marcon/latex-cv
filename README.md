# My curriculum vitae

## Prerequisites

The main prerequisite of this project is a LaTeX distribution.
This is currently tested with [TeX Live](https://tug.org/texlive/) on Microsoft Windows 11, but should also be compatible with any other recent distribution.
The main LaTeX package used is [moderncv](https://ctan.org/pkg/moderncv);
all other packages are fairly standard.

## Structure of repository

- [README.md](README.md): this file
- [CHANGELOG.md](CHANGELOG.md): a log of major changes
- [LICENSE.md](LICENSE.md): the LaTeX Project Public License (LPPL) version 1.3c file
- [src/](src): the source code
  - [us.tex](src/us.tex): a 1-page US résumé
  - [eu.tex](src/eu.tex): a 2-page european CV
  - [academic.tex](src/academic.tex): an extended academic CV
  - [auxiliary/](src/auxiliary): helper files for the main documents
  - [sections/](src/sections): section files for the main documents

## Build

At the moment, the CV files can be compiled using `latexmk`, e.g.:
```
cd src
latexmk -synctex=1 -interaction=nonstopmode -file-line-error -lualatex -outdir=. us.tex
```

Implementation of cross-platform build automation is underway with CMake.

## Acknowledgments

Special thanks to [Dave Moxey](https://github.com/mdave) for some of his comments and feedback.