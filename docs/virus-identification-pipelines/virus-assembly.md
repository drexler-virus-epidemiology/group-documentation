# Virus Assembly Pipeline

Steps:
* Read extraction (diamond)
* Assemble extracted reads (SPades)
* Index contigs (bowtie2)
* Map to assembly (bowtie2, samtools)
* Sort bam-files (samtools)
* Create output (QUAST)
