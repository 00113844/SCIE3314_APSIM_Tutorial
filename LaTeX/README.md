# SCIE3314 APSIM Tutorial — LaTeX Structure

## Overview

This directory contains the LaTeX source files for the SCIE3314 APSIM Tutorial. The structure is modular to avoid memory issues when working on large documents.

## Directory Structure

```
LaTeX/
├── main.tex                    # Root document (includes all modules/exercises)
├── preamble.tex               # Packages, formatting, custom commands
├── modules/
│   ├── module_0_why_apsim.tex
│   ├── module_1_structure.tex
│   ├── module_2_first_simulation.tex
│   ├── module_3_output_analysis.tex
│   ├── module_4_nitrogen_fertilisation.tex
│   ├── module_5_phenology_thermal_time.tex
│   ├── module_6_long_term_variability.tex
│   └── module_7_climate_scenarios.tex
├── exercises/
│   ├── exercise_a_fallow_water_balance.tex
│   ├── exercise_b_nitrogen_response.tex
│   ├── exercise_c_sowing_date.tex
│   └── exercise_d_20_year_variability.tex
├── appendix/
│   ├── appendix_a_components_reference.tex
│   └── appendix_b_troubleshooting.tex
├── figures/
│   └── [image placeholders and figures to be added]
└── README.md                  # This file
```

## Compilation Instructions

### Requirements

- LaTeX distribution: MiKTeX (Windows), MacTeX (macOS), or TeX Live (Linux)
- Recommended editor: TeXstudio, Overleaf, or VS Code + LaTeX Workshop

### Building the Document

#### Option 1: Command Line (Windows)

```bash
cd C:\Users\00113844\Documents\SCIE3314_APSIM_Tutorial\LaTeX
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

#### Option 2: Using latexmk (Recommended)

```bash
latexmk -pdf main.tex
```

#### Option 3: Overleaf

Upload all files to Overleaf; set main.tex as root document; compile.

### Output

- **main.pdf** — Complete tutorial (all modules, exercises, appendices)
- **main.aux, main.toc, main.log** — Intermediate files (can be deleted)

## Module Development Workflow

### Adding Content to an Existing Module

1. Open the module file (e.g., `modules/module_3_output_analysis.tex`)
2. Edit content sections
3. **Do not modify** the `\input{}` line in main.tex or chapter headers
4. Save module file
5. Recompile main.tex

### Creating a New Module

1. Create new file: `modules/module_N_name.tex`
2. Use template structure:
   ```latex
   \chapter{Module N: Title}
   \label{ch:moduleN}
   
   \section{Learning Objectives}
   % ... content ...
   
   \section{What's Next?}
   In Module N+1, ...
   ```
3. Add input line to main.tex:
   ```latex
   \input{modules/module_N_name}
   ```
4. Recompile

### Creating a New Exercise

1. Create new file: `exercises/exercise_X_name.tex`
2. Use Exercise A/B as template
3. Add input line to main.tex (in appendix section):
   ```latex
   \input{exercises/exercise_X_name}
   ```
4. Recompile

## Customization

### Changing Formatting

Edit `preamble.tex` to modify:
- Page geometry (margins, layout)
- Colors (darkblue, codegreen, etc.)
- Font sizes (see `\titleformat` commands)
- Code listing style (language, colors, borders)

### Adding Custom Commands

In `preamble.tex`, define new commands like:
```latex
\newcommand{\conceptbox}[1]{\begin{keyconceptbox}#1\end{keyconceptbox}}
```

Then use in modules:
```latex
\conceptbox{Your concept here}
```

### Changing Bibliography

Edit `preamble.tex`:
- Uncomment/modify `\addbibresource{references.bib}`
- Create `references.bib` file (BibTeX format)
- Recompile with `pdflatex → bibtex → pdflatex` cycle

## Common Tasks

### View Single Module (for Editing)

Create temporary file `test.tex`:
```latex
\input{preamble}
\begin{document}
\input{modules/module_1_structure}
\end{document}
```

Compile this to quickly preview one module without full document overhead.

### Adjust Line Spacing

In `preamble.tex`, change:
```latex
\onehalfspacing  % Change to \doublespacing or \singlespacing
```

### Add Figures

1. Place image files in `figures/` directory
2. In module, use:
   ```latex
   \begin{figure}[H]
     \centering
     \includegraphics[width=0.7\textwidth]{figures/image_name.png}
     \caption{Description}
     \label{fig:image_name}
   \end{figure}
   ```
3. Reference: `\cref{fig:image_name}`

### Change Color Scheme

Edit color definitions in `preamble.tex`:
```latex
\definecolor{darkblue}{rgb}{0.0, 0.0, 0.5}
```

Change RGB values (0–1 range) to your preferred colors.

## Tips for Modular Development

1. **Compile Frequently**: After each section, recompile to catch errors early
2. **Use Labels**: Add `\label{}` to all chapters, sections, figures for cross-referencing
3. **Avoid Absolute Paths**: Use relative paths (modules/, exercises/) for portability
4. **Backup**: Keep version control (git) or regular backups
5. **Test Cross-References**: Use `\cref{}` and `\ref{}` to link between modules

## File Size Management

- **Each module**: ~50–100 KB (manageable)
- **Each exercise**: ~100–150 KB (manageable)
- **Full document PDF**: ~5–10 MB (reasonable)

If document becomes slow to compile:
1. Check for large image files (compress to <1 MB each)
2. Consider splitting into separate PDFs (one per module)
3. Use `\includeonly{}` to compile subset of modules during drafting

## References and Bibliography

If using citations:

1. Create `references.bib` in LaTeX/ directory:
   ```bibtex
   @article{Holzworth2014,
     author = {Holzworth, D. P. and ...},
     title = {APSIM -- Evolution towards a new generation...},
     journal = {Environmental Modelling \& Software},
     year = {2014}
   }
   ```

2. Cite in text: `\cite{Holzworth2014}`

3. Compile cycle: `pdflatex → bibtex → pdflatex → pdflatex`

## Troubleshooting LaTeX Errors

### "Undefined control sequence"
- Check spelling of commands (\centering, \textbf, etc.)
- Ensure packages loaded in preamble.tex

### "File not found"
- Verify relative path to included files
- Check file names match (case-sensitive on Linux/Mac)

### "Overfull \hbox"
- Long lines exceed margin; add line breaks or use smaller font

### PDF won't compile
- Check `.log` file for specific error
- Comment out problematic sections; recompile incrementally

## Git Integration

Use git to version control LaTeX files:

```bash
cd LaTeX/
git init
git add .
git commit -m "Initial LaTeX structure"
git remote add origin https://github.com/your-username/scie3314-tutorial.git
git push -u origin main
```

Pull latest version before editing:
```bash
git pull origin main
```

## Next Steps

1. **Content Development**: Fill in placeholder sections (marked with `\vfill` and `\textit{[To be completed]}`)
2. **Figure Generation**: Add diagrams/plots to `figures/` directory
3. **Testing**: Run each exercise with provided near-solved .apsimx files
4. **Peer Review**: Share PDF for feedback
5. **Publication**: Export to other formats (HTML, EPUB) if needed

---

**Maintained by**: SCIE3314 Tutorial Development Team  
**Last Updated**: May 1, 2026  
**Version**: 1.0
