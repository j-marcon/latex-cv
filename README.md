# My curriculum vitae

## Prerequisites

The main prerequisite of this repository is a LaTeX distribution.
This project is currently tested against [TeX Live](https://tug.org/texlive/) on Microsoft Windows 11, but should also be compatible with any other recent distribution.
The main LaTeX package used is [moderncv](https://ctan.org/pkg/moderncv);
all other packages are fairly standard.

[CMake](https://cmake.org/) is used for cross-platform build automation;
however, the documents may also be compiled with `latexmk` or directly with LuaLaTeX.

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

The documents may be compiled using the standard `CMake` build procedure, e.g. running from the root directory:
```
cmake -S . -B build
cmake --build build --parallel
```
To compile only one of the main documents, its name can be passed as a target to the second command, e.g.:
```
cmake --build build --target us
```

Alternatively, the documents may be compiled using `latexmk`, e.g.:
```
cd src
latexmk -lualatex us.tex
```

## Acknowledgments

Special thanks to [Dave Moxey](https://github.com/mdave) for his feedback and help.