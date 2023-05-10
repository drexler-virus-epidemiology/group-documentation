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
r_on_cluster <- TRUE
if (r_on_cluster == TRUE) {
  bp <- "/data/gpfs-1/users/cabe12_c"
  .libPaths("/fast/work/groups/ag-drexler/RPackageLibrary/4.1.3/")
} else {
  if (grepl(x = getwd(), pattern =  "carl")) {
    bp <- "/home/carl/Dokumente/06_projects/"
  }
}

#+ load.packages, include=F
for (i in c(
  "data.table",
  "here",
  "Hmisc",
  "ggplot2",
  "ggrepel",
  "ggthemes",
  "Rqc",
  "QuasR"
  )
  ) {
  suppressPackageStartupMessages(
    library(i, character.only = TRUE
    ))
  }

checkForUpdates <- FALSE
if (checkForUpdates) {
  
  # set a more recent R snapshot as source repo
  r = getOption("repos")
  r["CRAN"] = "https://mran.microsoft.com/snapshot/2022-12-08"
  options(repos = r)
  rm(r)
  
  # Check unsuccessful updates packages
  old.packages()

  # Update packages to that snapshot
  update.packages(
    ask = FALSE,
    checkBuilt = TRUE
  )
}

# ggplot theme
ggplot2::theme_set(
  theme_tufte(base_size = 14) +
    theme(panel.background = element_rect(colour = "grey35"))
)

# Knitr should use the project root and not the script location as root
knitr::opts_knit$set(root.dir = here(), 
                     base.dir = here())

# Give data.table enough threads
writeLines(paste0("Threads available: ", parallel::detectCores()))
writeLines(paste0("Threads given to data.table: ", parallel::detectCores() / 2))
setDTthreads(parallel::detectCores() / 2)

# Option setup for
options(prType = 'html')
options(knitr.table.format = "html")
options(grType = 'plotly')
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

