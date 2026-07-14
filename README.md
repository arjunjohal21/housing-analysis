# housing-analysis

**Did COVID-19 Drive a Suburban Housing Boom?**

A difference-in-differences analysis of whether the COVID-19-era shift toward remote work caused suburban county housing prices to rise faster than urban core county housing prices.

## Research question

Did the COVID-19 shift toward remote work lead to a relative increase in housing prices in suburban counties compared to urban core counties?

## Data

- **FHFA House Price Index** — county-level annual panel of repeat-transactions housing price indices from the Federal Housing Finance Agency.
- **CDC NCHS Urban-Rural Classification Scheme** — a static, county-level 6-tier classification (1 = Large Central Metro to 6 = Non-core) used to define treatment/control groups.

The two datasets are merged on 5-digit county FIPS codes. Large Central Metro counties (`CODE2023 == 1`) serve as the urban core **control** group; Large Fringe Metro counties (`CODE2023 == 2`) serve as the suburban **treatment** group.

## Method

Difference-in-differences Design:

- **Sample:** 2010–2024, restricted to urban core and suburban counties, balanced panel (each county must have all 15 annual observations).
- **Outcome:** winsorized year-over-year HPI growth rate (`chg_w`), not raw HPI levels, since FHFA's index is anchored to county-specific base years and isn't comparable across counties.
- **Treatment indicator:** `did_term = suburban x post_covid`, where `post_covid = 1` for years ≥ 2020.
- **Identifying assumption:** parallel trends between suburban and urban core growth rates absent COVID-19, assessed visually pre-2020.

## Repo Contents

| File | Description |
|---|---|
| [housing_analysis.Rmd](housing_analysis.Rmd) | Full write-up: data description, sample construction, DiD analysis, and robustness checks (R Markdown) |
| [housing_analysis.pdf](housing_analysis.pdf) | Rendered PDF of the analysis |
| [slides.qmd](slides.qmd) | Presentation deck summarizing the question, method, findings, and recommendation (Quarto) |

## Reproducing

The `.Rmd` downloads both source datasets directly from Dropbox at render time. Requires R with `tidyverse`, `readxl`, `scales`, `knitr`, and `kableExtra`, plus a working `xelatex` install for PDF output:

```r
rmarkdown::render("housing_analysis.Rmd")
```

The slide deck can be rendered with [Quarto](https://quarto.org/):

```bash
quarto render slides.qmd
```
