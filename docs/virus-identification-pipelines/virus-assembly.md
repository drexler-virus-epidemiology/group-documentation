# Virus Assembly Pipeline

## Quick Start

After selecting taxa and entering them into the config file...

Run the assembly pipeline until read extraction

```bash
snakemake -s virus_assembly.snakemake \
    --use-conda \
    --cores=8 \
    --conda-frontend mamba \
    --configfile 02.05-assemblyConfigTemplate/\
                 assemblyConfigTemplate_2_Samples.yaml
    --until diamondReadExtraction

```

To start the read extraction and continue with assembly (some explanation
necessary). Thee `--keep-going` flag skips samples where no contigs could be
generated from the reads.

```bash 
snakemake -s virus_assembly.snakemake \
    --profile cubi-v1 \
    --configfile 02.05-assemblyConfigTemplate/assemblyConfigTemplate_3_Samples.yaml \
    --keep-going \
    --jobs 10
```

## Steps and used tools

### Steps

* Read extraction (diamond)
* Assemble extracted reads (SPades)
* Index contigs (bowtie2)
* Map to assembly (bowtie2, samtools)
* Sort bam-files (samtools)
* Create output (QUAST)

### Used file formats

* fastq.gz
* BAM/SAM
* fastp, [SPades](https://github.com/ablab/spades), DIAMOND,

### Output

1. An assembly in Fasta format 
2. A mapping of the extracted reads to the assembly
3. A quast report summerizing the assembly
