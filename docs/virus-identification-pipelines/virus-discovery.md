# Virus Discovery Pipeline

## Quick Start

Create a conda environment with several packages

`snakemake`, `mamba`

It may be necessary to install several other packages: `perl`, `libxcrypt`.
Maybe the system library `libxcrypt-compat` might be necessary in case you
receive the error:

``` bash 
snakemake -s virus-discovery.snakemake \
    --use-conda \
    --cores=8 \
    --conda-frontend mamba \
    --config "readfolder=~/Dokumente/02_raw_data/\
              230405_virus-discoveryPipelineTestData/"\
              "db=/home/carl/Dokumente/07_databases/\
               2023-03-13_refseqViral_2.0.14/" 

```

Select taxa and enter them into the configuration file.

## Inputs

* Reads
* Database

## Steps and used tools

* DB Check (and download)
* Lane merging
* Trimming (fastp)
* Merge paired (?) reads
    * What is R1, R2, R1up, R2up?
* Allign DNA sequences against protein reference DB (diamond blastx)
* Something about NCBI taxonomy (Diamond Sluice Python script)
    * Turn Diamond Output into Readgroups asigned to Taxids
    * Allign Reads and Taxon IDs for the Krona PLot
* Update the Krona Taxonomy (krona)
* Create the Krona plot (krona)
* Create YAML configuration for the assembly pipeline

## Quality metrics

* Contig length
* runtime
* 

## Comparison with VPipe 

* Mapping, trimming & de-duplication of primers and adapters (cutadapt + custom
  script dedup.py)
* Host removal (Bowtie2)
* Two modules
    * de-novo assembly (SPAdes) & virus detection (BLASTn)
    * reference-based assembly
