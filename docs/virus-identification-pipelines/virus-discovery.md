# Virus Discovery Pipeline

## Quick Start

Create a conda environment with several packages

`snakemake`, `mamba`

It may be necessary to install several other packages: `perl`, `libxcrypt`.
Maybe the system library `libxcrypt-compat` might be necessary in case you
receive the error:

``` bash
snakemake -s virusDiscovery.snakemake \
    --use-conda \
    --cores=8 \
    --conda-frontend mamba \
    --config "readfolder=~/Dokumente/02_raw_data/\
              230405_virusDiscoveryPipelineTestData/"\
              "db=/home/carl/Dokumente/07_databases/\
               2023-03-13_refseqViral_2.0.14/" 

```

Select taxa and enter them into the configuration file.

Run the assembly pipeline. 

```bash
snakemake -s virus_assembly.snakemake \
    --use-conda \
    --cores=8 \
    --conda-frontend mamba \
    --configfile 02.05-assemblyConfigTemplate/\
                 assemblyConfigTemplate_2_Samples.yaml
    --until diamondReadExtraction

```


## Steps and used tools

* fastp, [SPades](https://github.com/ablab/spades), DIAMOND,
