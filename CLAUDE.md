# CLAUDE.md — AI Assistant Guide

## Project Overview

Business Forecasting course materials (6 chapters) authored by Krzysztof Zaremba. The repository contains xaringan slide decks, interactive learnr tutorials, exam sheets, and a Bookdown documentation site — all built with R Markdown.

## Repository Structure

```
C1/ – C6/          Chapter folders (slides, tutorials, data, compiled output)
Intro_to_r/         Beginner R scripts and datasets
Exam_sheets/        Formula and code reference PDFs
docs/               Bookdown HTML book output
```

Each chapter folder follows a consistent layout:
- `C_N_slides[_variant].Rmd` — xaringan presentation source
- `C_N_learnr[_solved].Rmd` — interactive learnr tutorial
- `R_script_CN.R` / `Prepare_data.R` — utility scripts
- `Data/` — chapter datasets (`.Rda`, `.csv`)
- `libs/` — compiled JS/CSS widget dependencies
- `my-css.css` — shared custom CSS classes
- `xaringan-themer.css` — auto-generated theme (do not edit by hand)

Chapters 3–6 have sub-folders (`C3_a/`, `C3_b/`, etc.) for multi-part lectures.

## Technology Stack

| Layer | Tools |
|-------|-------|
| Language | R |
| Slides | xaringan + xaringanthemer |
| Tutorials | learnr (Shiny runtime) |
| Book | Bookdown (GitBook format, output in `docs/`) |
| Visualization | ggplot2, plotly, leaflet, DT |
| Data wrangling | dplyr / tidyverse |
| PDF export | `pagedown::chrome_print()` (see `To_pdf.R`) |

There is no formal package manager file (`DESCRIPTION`, `renv.lock`). Dependencies are loaded inline via `library()` in each Rmd's setup chunk.

## Build / Render Commands

```r
# Render slides to HTML
rmarkdown::render("C1/C_1_slides.Rmd")

# Convert HTML slides to PDF
pagedown::chrome_print("C1/C_1_slides.html", output = "C1/C_1_slides.pdf", timeout = 9000)

# Run an interactive learnr tutorial
rmarkdown::run("C2/C_2_learnr_data_types_visualizations.Rmd")
```

## Key R Packages (≈30 total)

**Presentation:** xaringan, xaringanthemer, knitr, kableExtra
**Interactive:** learnr, shiny, DT
**Visualization:** ggplot2, plotly, leaflet, ggdist, gridExtra, cowplot, patchwork
**Data:** dplyr, tidyverse, tidyr, reshape2, readr, lubridate
**Stats / Forecasting:** MASS, forecast, fpp3, feasts, tsibble, tsibbledata, car
**Teaching datasets:** Lock5Data, wooldridge, gapminder, probstats4econ

## Conventions & Patterns

### Rmd structure
Every Rmd starts with:
1. YAML front-matter specifying `xaringan::moon_reader` (slides) or `learnr::tutorial` (tutorials).
2. A `setup` chunk (`include=FALSE`) that loads libraries and data.
3. A `xaringan-themer` chunk for theme colors (slides only).

### Custom CSS classes (defined in `my-css.css`)
- Text colors: `.blue`, `.red`, `.pink`, `.green`, `.orange`, `.purple`
- Highlighting: `.hi`, `.hi-pink`, `.hi-slate`
- Size: `.bigger`, `.smaller`, `.smallest`
- Layout: `.inverse`, `.mono`, `.footnote`

### Naming
- Slide files: `C_N_slides_[variant].Rmd`
- Tutorial files: `C_N_learnr[_solved].Rmd`
- Data folders always named `Data/`
- Compiled widget assets always in `libs/`

### Data loading
```r
load("Data/Health_data.Rda")          # .Rda files
data <- read.csv("data/file.csv")    # CSV files
```

## What NOT to Edit

- `xaringan-themer.css` — auto-generated; modify the `xaringan-themer` chunk instead.
- `libs/` directories — compiled output from knitr; regenerated on render.
- `.html` files alongside `.Rmd` — these are rendered output, not source.
- `docs/` — Bookdown output; rebuild from source Rmd files.

## Testing

No formal test suite. Quality is verified by:
1. Successfully rendering each Rmd to HTML without errors.
2. Running learnr tutorials and checking interactive exercises.
3. Reviewing PDF output from `pagedown::chrome_print()`.

## Git Notes

- `.gitattributes` enforces LF line endings (`* text=auto`).
- Large binary files (HTML with embedded widgets, PDFs, `.Rda`) are committed directly — no Git LFS.
- Repository is ~716 MB due to compiled output and data files.
