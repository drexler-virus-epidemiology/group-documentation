# Data Organisation

!!! todo
    This site is dedicated to how data (data sets as well as analysis scripts
    etc.) are to be organized and documented. A good place to look on how to
    organize large projects are guidelines for large programming projects like
    the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/). For
    many tasks, we need meta-data and naming conventions.

## File System

```bash
imise
├── 00_archiv
├── 01_daten
├── 02_projekte
├── 02_qcdaten
├── 03_aussendarstellung
├── 04_archiv
├── 05_literatur
├── 06_technik
├── 07_programme
├── 08_antraege
├── 09_nutzer
├── 10_datenmanagement
├── 11_management
├── 12_konzepte
├── 13_lehre
├── 14_ideen
├── 15_anfragen
```

## Analysis Project

### Directory Structure

```bash
YYMM_cohort_analysis_trait/
├── archive
├── data
├── documents
├── literature
├── paper
├── results
├── scripts
├── sent
└── temp
```

### Readme Content

### Script Organization

* No paths in the script itself, use command line arguments or a configuration
  file, e. g. YAML with entries like:
    * `REFSEQ_DB: "/INPUT/DIR/PATH/"`

## Raw Data Repository

* Readme
* Raw data
* Cleaned raw data
* Sample annotation (incl. data dictionary)
* Feature annotation (incl. data dictionary)

## QC Data Repository

* Summary
* Folders & Files description
* Sample & Feature annotation (incl. data dictionary)
* Derived data (quantities computed from raw data)
* For papers
    * Methods (collection, pre-treatment, measurement etc.)
    * Relevant papers to cite (methods, studies etc.)
    * Ethics statement
    * Funding statement
    * Acknowledgement
