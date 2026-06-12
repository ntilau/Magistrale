# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Master's thesis on **Model Order Reduction (MOR) in Finite Element Method (FEM) analysis of phased array antennas**. The thesis covers: FEM formulation for Maxwell's equations, near-field to far-field (N2F) transformation using DFT truncation, and projection-based MOR via Proper Orthogonal Decomposition (POD/SVD) to reduce computational cost for scanning radiation patterns.

The defense presentation (in Italian) includes timing results not present in the thesis manuscript.

## Build System

```bash
make                 # Build both thesis and presentation
make manuscript      # Build only manuscript/thesis.pdf
make presentation    # Build only presentation/defense.pdf
make clean           # Remove LaTeX temp files (keep PDFs)
make distclean       # Remove temp files AND PDFs
make help            # Show all targets and variables
```

Print a specific page range of the thesis (requires `make` first):
```bash
cd manuscript && pdflatex "\includeonly{chapter2}\input{thesis}" && open thesis.pdf
```

### Build Configuration

All variables have defaults, override with `make VAR=value`:

| Variable | Default | Notes |
|----------|---------|-------|
| `MS_TEX` | `pdflatex` | LaTeX engine for manuscript |
| `MS_TEXFLAGS` | `-interaction=nonstopmode -file-line-error` | |
| `MS_BIB` | (empty) | Set to `biber` or `bibtex` if bibliography needed |
| `MS_FILE` | `thesis` | Base name (no `.tex`) |
| `PR_TEX` | `pdflatex` | LaTeX engine for presentation |
| `PR_TEXFLAGS` | `-shell-escape -interaction=nonstopmode -file-line-error` | `-shell-escape` needed for Beamer multimedia |
| `PR_BIB` | (empty) | Set to `biber` or `bibtex` if bibliography needed |
| `PR_FILE` | `defense` | Base name (no `.tex`) |

The build runs **three LaTeX passes** (with optional bibliography step after pass 1) to resolve cross-references, citations, and TOC page numbers.

### Dependencies

- `pdflatex` (TeX Live or similar distribution)
- `make` (build automation)
- Optional: `biber` or `bibtex` for bibliography

Run `./configure` to auto-install dependencies on macOS (brew), Debian/Ubuntu (apt), Arch (pacman), or Fedora/RHEL (dnf).

## Document Structure

### Manuscript (`manuscript/`)

Entry point: `manuscript/thesis.tex` — uses `memoir` document class, includes chapters via `\include`:

```
thesis.tex
├── title.tex            Title page
├── abstract.tex
├── acknowledgements.tex
├── contents.tex         TOC, switches to arabic page numbering
├── introduction.tex
├── chapter1.tex         N2F transformation theory
├── chapter2.tex         MOR theory (POD, SVD, reduced basis)
├── chapter3.tex         Numerical results (patch array, Vivaldi array)
├── conclusion.tex
└── bibliography.tex     Manual thebibliography environment (not BibTeX)
```

- `custom.tex` — Shared packages, math commands (`\vect`, `\mat`, `\mbfit`, `\dyad`, `\spanning`, `\colspan`, etc.), theorem environments, hyperref config, chapter style
- `img/` — Images organized by chapter: `img/ch1/`, `img/ch2/`, `img/ch3/`, plus logos in `img/`
- `\graphicspath{{img/}}` is set in thesis.tex, so images are referenced without path prefix
- Bibliography is manual (`\begin{thebibliography}`), not BibTeX-based — `MS_BIB` is empty by default for this reason
- Page numbering: roman for front matter, arabic from contents onward (`contents.tex` switches)
- The `natbib` package is loaded with `unsrtnat` style

### Presentation (`presentation/`)

Entry point: `presentation/defense.tex` — Beamer document class with Malmoe theme.
- `custom.tex` — Italian language, Beamer theme colors, math commands, `colorblock` environment for highlighted equations
- `img/` — Presentation figures, logos, plus `movscan.mp4` (animated scan video)
- `discorso.md` — Italian speech notes for the defense
- Uses `\include{custom}` (same pattern as manuscript)

### Simulations (`simulations/`)

FEM MOR numerical experiments, organized by project:

```
simulations/FEM_MOR_projects/
├── femSolvers/                  Compiled solver executables (Windows .exe)
│   ├── EM_WaveSolver.exe        FEM wave equation solver
│   ├── ANST_MeshReader.exe      HFSS mesh import tool
│   └── EM_EigenModeSolver2D.exe
├── 3x5patch_1stOrder/           15-element patch array, 1st-order basis
│   ├── *.m                      MATLAB MOR scripts (MOR_3x5patch.m, etc.)
│   ├── *.hfss                   HFSS project file
│   ├── *.bat                    Windows batch files to invoke solvers
│   └── *.mpara, *.seinfo, ...   Solver input files
├── 3x5patch_2ndOrder/           Same array, 2nd-order FEM basis
├── 40vivaldi/                   40-element Vivaldi antenna array
│   ├── MOR_40vivaldi_*_2Dscan.m
│   ├── MOR_40vivaldi_*_3Dscan.m
│   └── LumpedPortsOrder.m       Port ordering for scan excitations
└── 1x1vivaldi_LTE/             Single Vivaldi element, N2F validation
    └── test_N2F_HFSS.m          HFSS-to-MATLAB N2F comparison
```

Plus MATLAB N2F library and interpolation tests (scalar/vector field approximation, polynomial basis, DFT truncation).

### Papers (`papers/`)

Reference PDFs organized by topic:
- `Approximation/` — POD, reduced basis, empirical interpolation (DEIM) papers
- `FEM/` — FEM in electromagnetics, edge elements, adaptive meshing
- `MOR/` — Model order reduction methods

### Notes (`notes/`)

Supplementary materials: internship report (`ln_Thesis_Tirocinio5.pdf`) and related notes.

## LaTeX Tips

- **Selective chapter compilation**: Edit `thesis.tex` to comment out `\include` lines for chapters you're not working on, or use `\includeonly{chapter1,chapter2}` in the preamble
- **Font**: Uses `mathdesign` with Adobe Utopia and `isomath` for math fonts
- **Encoding**: `latin1` input encoding (not UTF-8), `T1` font encoding
- **Italic in Beamer frames**: Use `\itt{}` not `\textit{}` in the presentation (custom command)
- **Degree symbol in presentation**: `\acgrave{}` not `\`{}` (custom command renames it)
- **Presentation videos**: `-shell-escape` flag enables `\href{run:movscan.mp4}{}` links
- **Hyperref backref**: The manuscript uses `[backref=page]` via `hyperref` package, showing page numbers where citations appear
