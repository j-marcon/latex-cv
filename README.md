# My curriculum vitae

## Prerequisites

The main prerequisite of this repository is a LaTeX distribution.
This project is currently tested against [TeX Live](https://tug.org/texlive/), but should also be compatible with any other distribution.
The main LaTeX package used is [moderncv](https://ctan.org/pkg/moderncv);
all other packages are fairly standard.

[CMake](https://cmake.org/) v3.21 (or later) is used for cross-platform build automation;
however, the documents may also be compiled with `latexmk` or directly with LuaLaTeX.
On Microsoft Windows, the Developer PowerShell for [Visual Studio](https://visualstudio.microsoft.com/vs/) is needed alongside CMake.

This project has been tested on Microsoft Windows 11 as well as Ubuntu 18 (through the Windows Subsystem for Linux) and 20 (through the GitHub Actions virtual environments) but should work on any other system supported by CMake with a compatible LaTeX distribution.

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

The documents may be compiled using the standard `CMake` build procedure.
If `CV_ROOT` is the path to this repository, the build procedure would look like this:
```sh
cd $CV_ROOT
cmake -S . -B build            # generate the project build system
cmake --build build --parallel # build the project
cmake --install build          # install the project
```
The last command will install the PDF documents in a `bin` directory by default.
The `--install-prefix <dir>` option may be used with the generate command or `--prefix <dir>` with the install command to change the installation directory.

To compile only one of the main documents, its name can be passed as a target to the build command, e.g.:
```sh
cmake --build build --target us
```

Alternatively, the documents may be compiled using `latexmk`, e.g.:
```sh
cd src
latexmk -lualatex us.tex
```
This method is not actively tested but should work.

## Acknowledgments

Special thanks to [Dave Moxey](https://github.com/mdave) for his feedback and help.