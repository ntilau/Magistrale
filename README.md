# Univ-Master

LaTeX sources for a Master's thesis manuscript and its defense presentation.

## Repository Layout

```
manuscript/          Main thesis document
├── thesis.tex       Entry point (includes all chapters via \include)
├── title.tex        Title page
├── abstract.tex     Abstract
├── acknowledgements.tex
├── introduction.tex
├── chapter1.tex     Content chapters
├── chapter2.tex
├── chapter3.tex
├── conclusion.tex
├── bibliography.tex Bibliography definitions
├── custom.tex       Custom commands and package configuration
├── img/             Images organized by chapter
│   ├── ch1/         Chapter 1 figures (PDF)
│   ├── ch2/         Chapter 2 figures (PDF)
│   ├── ch3/         Chapter 3 figures (PDF)
│   └── *.pdf        Logos and shared resources
└── [thesis.pdf](manuscript/thesis.pdf) Generated output

presentation/        Defense presentation (Beamer slides)
├── defense.tex      Entry point for Beamer
├── custom.tex       Beamer theme and package customization
├── img/             Presentation images and diagrams
├── movscan.mp4      Embedded video animation
└── [defense.pdf](presentation/defense.pdf) Generated output

Makefile             Build orchestration (supports make all, manuscript, presentation, clean, distclean, help)
README.md            This file
configure            Automated dependency installer script
```

## Requirements

- `make` - build automation
- `pdflatex` - LaTeX engine (included in most TeX distributions)
- **Optional**: `biber` or `bibtex` for bibliography processing

Install dependencies on macOS, Debian/Ubuntu, Arch, Fedora/RHEL, and other supported systems:

```bash
./configure
```

This script automatically detects your system and installs required packages.

## Quick Start

Build both documents:

```bash
make
```

Build only the thesis manuscript:

```bash
make manuscript
```

Build only the defense presentation:

```bash
make presentation
```

View results:

- [manuscript/thesis.pdf](manuscript/thesis.pdf) - Thesis PDF
- [presentation/defense.pdf](presentation/defense.pdf) - Presentation PDF

## Build Process

The build system compiles LaTeX documents in multiple passes to resolve cross-references and citations:

1. **LaTeX compilation**: Initial document processing
2. **Bibliography** (if configured): Process bibliography database (optional)
3. **LaTeX compilation (pass 2)**: Update citations and references
4. **LaTeX compilation (pass 3)**: Finalize all references

### Three-pass compilation ensures:
- ✓ All cross-references are resolved
- ✓ Table of contents is accurate
- ✓ Citations and bibliography are properly formatted
- ✓ Page numbers in references reflect final layout

## Cleaning Up

Remove temporary LaTeX files (keeps generated PDFs):

```bash
make clean
```

Remove temporary files AND generated PDFs:

```bash
make distclean
```

Temporary files cleaned: `*.aux`, `*.log`, `*.toc`, `*.nav`, `*.out`, `*.bbl`, etc.

## Configuration

Override default settings from the command line:

```bash
make VAR=value target
```

### Manuscript variables
- `MS_FILE` - Main source file without `.tex` extension (default: `thesis`)
- `MS_TEX` - LaTeX engine command (default: `pdflatex`)
- `MS_TEXFLAGS` - Compiler flags (default: `-interaction=nonstopmode -file-line-error`)
- `MS_BIB` - Bibliography processor: `biber`, `bibtex`, or leave empty for none (default: empty)

### Presentation variables
- `PR_FILE` - Main source file without `.tex` extension (default: `defense`)
- `PR_TEX` - LaTeX engine command (default: `pdflatex`)
- `PR_TEXFLAGS` - Compiler flags (default: `-shell-escape -interaction=nonstopmode -file-line-error`)
- `PR_BIB` - Bibliography processor (default: empty)

### Examples

Build manuscript with Biber bibliography:

```bash
make MS_BIB=biber manuscript
```

Build with different source file names:

```bash
make MS_FILE=mainThesis PR_FILE=presentation
```

List all available targets:

```bash
make help
```

## Project Structure Details

### Manuscript Organization

The main thesis file (`manuscript/thesis.tex`) uses `\include` commands to import chapters modularly:

```latex
\include{title}
\include{abstract}
\include{introduction}
\include{chapter1}
\include{chapter2}
\include{chapter3}
\include{conclusion}
```

This modular structure allows:
- Selective compilation of individual chapters during editing
- Easy reordering of chapters
- Clean separation of concerns

### Custom Commands

Both documents include `custom.tex` for:
- Common package imports
- Custom command definitions
- Document-specific configurations

### Image Management

Images are organized by chapter to maintain clarity:

- Chapter-specific images in `img/ch1/`, `img/ch2/`, etc.
- Shared resources (logos) in `img/`
- All images use lowercase filenames for consistency across platforms

## Troubleshooting

**"Make: command not found"**
- macOS: `brew install make`
- Ubuntu/Debian: `apt install build-essential`
- Other systems: See your package manager

**"pdflatex: command not found"**
- Install a TeX distribution: `./configure` or manually install TeX Live

**Bibliography not appearing**
- Ensure `MS_BIB` or `PR_BIB` is set to `biber` or `bibtex`
- Example: `make MS_BIB=biber manuscript`

**PDF not updating**
- Run `make distclean` to remove all cache, then `make` to rebuild

## Outputs

Generated files:

- [manuscript/thesis.pdf](manuscript/thesis.pdf) - Full thesis document
- [presentation/defense.pdf](presentation/defense.pdf) - Presentation slides
