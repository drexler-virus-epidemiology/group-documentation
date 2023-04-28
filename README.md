# Virus Epidemiology Group Documentation

This repository contains documentation on a collection of topics that are
relevant for members of the [Drexler virus epidemiology
laboratory](https://virologie-ccm.charite.de/forschung/ag_drexler/). 

## Download the repository

The repository ist hosted on GitLab and GitHub. The GitHub instance should be
preferred, the GitLab is a legacy mirror.

``` bash
# From GitHub
git clone git@github.com:drexler-virus-epidemiology/group-documentation.git

# From GitLab 
git clone ssh://git@git-ext.charite.de:2022/drexler-virus-epidemiology/group-documentation.git
```

## Change directory into the repository

``` bash
cd group-documentation
```

## If you don't have MKdocs installed (but conda/mamba)

``` bash
conda create -n mkdocs mkdocs
conda activate mkdocs
```

## Display the HTML in the browser

``` bash
mkdocs serve
```

## Create the HTML in the site/ directory

``` bash
mkdocs build
```
