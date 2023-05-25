# Samtools Alignment Check & Filter

## What

Here we show how to summarize the bitwise flags that contain properties of the
alignment of each read. Reads can be filtered using these flags.

## Why 

After using `bowtie2` to create an alignment of reads to a reference,
`samtools` can be used to inspect the resulting SAM/BAM file and create
summaries of the alignment process. Aligment files (SAM/BAM) contain
information about each read that is contained in the bitwise flag. `Samtools`
can be used to access this information and the GNU utils can be used to
aggregate this data. Filtering reads using these flags can be used to collect
reads that did not match to a specific reference, e. g. the host or a
contamination of no further interest.

## How

Print an overview of the flags' meaning:

```
# See explanation of all flags
samtools flag

# See explanation of a specific flag (especially aggregate flags)
samtools flag <MY_FLAG>
```

You can for example search for and count all reads that has a reference
assigned in the SAM header by filtering for headers without a `*` in the third
position. The same can be reversed to count reads with no assigned reference.

```
# Search for reads with assigned reference
ssamtools view <PATH/TO/SAM/FILE.sam> | \
    awk -F'\t' '$3 != "*" {print $1, $2, $3}' | \
    wc -l

# Search for reads without an assigned reference
amtools view <PATH/TO/SAM/FILE.sam> | \
    awk -F'\t' '$3 == "*" {print $1, $2, $3}' | \
    wc -l
```

Count the lines in the alignment file where the read was aligned to a reference. 
Display all the bitflags of all (un-)aligned reads. Add the filter from above
to only display aligned (`$3 != "*"`) or unaligned reads (`$3 == "*"`).

```
# Display all flags in the file
samtools view <PATH/TO/SAM/FILE.sam> | \
    awk -F'\t' '{flags[$2] = 1} END {for (flag in flags) print flag}' | \
    while read -r flag; do samtools flag "$flag"; done

# Display flags of reads with an assiged reference ($3 != "*")
samtools view <PATH/TO/SAM/FILE.sam>  | \
    awk -F'\t' '$3 != "*" {flags[$2] = 1} END \
    {for (flag in flags) print flag}' | \
    while read -r flag; do samtools flag "$flag"; done

# Output of the command:
#> 0x49    73      PAIRED,MUNMAP,READ1
#> 0x51    81      PAIRED,REVERSE,READ1
#> 0x53    83      PAIRED,PROPER_PAIR,REVERSE,READ1
#> 0x59    89      PAIRED,MUNMAP,REVERSE,READ1
#> 0x61    97      PAIRED,MREVERSE,READ1
#> 0x63    99      PAIRED,PROPER_PAIR,MREVERSE,READ1
#> 0x65    101     PAIRED,UNMAP,MREVERSE,READ1
#> 0x85    133     PAIRED,UNMAP,READ2
#> 0x91    145     PAIRED,REVERSE,READ2
#> 0x93    147     PAIRED,PROPER_PAIR,REVERSE,READ2
#> 0x99    153     PAIRED,MUNMAP,REVERSE,READ2
#> 0xa1    161     PAIRED,MREVERSE,READ2
#> 0xa3    163     PAIRED,PROPER_PAIR,MREVERSE,READ2
#> 0xa5    165     PAIRED,UNMAP,MREVERSE,READ2
```

Interestingly, event though reads have an assigned reference, some still
contain the `UNMAP` (i. e. `4`) property. `samtools view -b -f` can then be
used to select (using the `-f` flag) all unmapped reads and write them into a
BAM file (using the `-b` flag). Reversing the filter is done using the `-F`
flag.

```
# Select only unmapped reads (UNMAP flag, i. e. 4)
samtools view -b -f 4 <PATH/TO/SAM/FILE.sam> > unmapped.bam

# Select only mapped reads (UNMAP flag, i. e. 4) by reversing the filter
samtools view -b -F 4 <PATH/TO/SAM/FILE.sam> > mapped.bam
```

## Results

Separate SAM/BAM files for the reads based on the filter.

## Further Reading

* Book: Vince Buffalo - Bioinformatics Data Skills
