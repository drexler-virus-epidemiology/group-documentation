# Using R

!!! todo
    This site is dedicated to common use-cases of the R programming language.

```r
# This is an R chunk
library(data.table)
```

## RStudio Projects

* Relative Paths
* Report generation: Using Quarto (YAML-Preset von mir)
* Best Practices: SessionInfo, no rm(ls)...
* Good Presets (no cache)
* Markup scripts

## Package library

## Local environments

* Use `renv`

## Useful packages

* [`rrtools`](https://github.com/benmarwick/rrtools)

## Useful resources

* https://happygitwithr.com/

## Session setup

```r 
#| echo: true
#| include: false

# define alternative package directory
r_on_server <- TRUE
if (r_on_server == TRUE) {
  bp <- "/net/ifs1/san_projekte/projekte/genstat/"
  computer <- "angmar"
  .libPaths(
    paste0(
      bp,
      "07_programme/rpackages/",
      computer
    )
  )
  # set number of cores
  nc <- 10
} else {
  if (grepl(x = getwd(), pattern =  "carl")) {
    bp <- "/home/carl/imise/"
    
    # set a more recent R snapshot as source repo
    r = getOption("repos") 
    r["CRAN"] = "https://mran.microsoft.com/snapshot/2022-12-08"
    options(repos = r)
    rm(r)
    
  } else if (grepl(x = getwd(), pattern = "J:/", fixed = TRUE)) {
    bp <- "J:/genstat/"
  } else {
    bp <- "/net/ifs1/san_projekte/projekte/genstat/"
  }
  # set number of cores
  nc <- parallel::detectCores() - 1
}
#+ load.packages, include=F
for (i in c(
  "data.table",
  "here",
  "readxl",
  "ggplot2",
)){
  suppressPackageStartupMessages(
    library(i, character.only = TRUE, lib.loc = .libPaths()[1]
    ))}

# Check unsuccessful updates packages
# old.packages()

# Update packages to that snapshot
# update.packages(
#   ask = FALSE, 
#   checkBuilt = TRUE
# )

# ggplot theme
ggplot2::theme_set(
  theme_tufte(base_size = 14)  + 
    theme(panel.background = element_rect(colour = "grey35")
    )
)
```

## Quarto

### YAML Header

```yaml
---
title: "My analysis"
author: "Carl Beuchel"
date: today
output:
  html_document:
  code_download: true
format:
  html:
    df-print: kable
    fig-width: 8
    fig-height: 6
    code-fold: true
    code-summary: "Show the code"
theme: spacelab #sandstone #flatfly #spacelab
highlight: pygments
toc: true  
toc-depth: 3
number-sections: true
toc-float:
  smooth-scroll: true
execute:
  include: true
  eval: true
  echo: true
  warning: false
editor: source
editor_options: 
  chunk_output_type: console
---
``` 

