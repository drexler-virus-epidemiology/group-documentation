# BCL to FASTQ conversion

## What

This document describes the necessary steps to convert BCL files to FASTQ files.

## Why

According to [Illumina
documentation](https://emea.illumina.com/informatics/sequencing-data-analysis/sequence-file-formats.html),
several sequencing technologies (NextSeq, HiSeq Sequencing Systems, NovaSeq
6000) generate binary base call (BCL) files. Most bioinformatics tools require
the FASTQ file format.

## How

The conversion is facilitated using Illumina software `bcl2fastq2` or the newer
version `BCL Convert`. Documentation and downloads are available
[here](https://support.illumina.com/sequencing/sequencing_software/bcl-convert.html).
The migration to `BCL Convert` is documented
[here](https://knowledge.illumina.com/software/general/software-general-reference_material-list/000003710).
Since `BCL Convert` is not available in any conda repository, the easiest way
is to install the `bcl2fastq2` software using a custom conda environment.
Otherwise, download the latest version of `BCL Convert` from the Illumina
[website](https://support.illumina.com/downloads/bcl-convert-v4-1-5-installer.html)
as an `.rpm` package, extract it and use the binary found in
`./usr/bin/bcl-convert`

Follow these steps:

1. Transfer the complete folder containing all received data and the `.csv`
   file to a location on the HPC cluster.
2. Either write a job script or start a terminal on a compute node.
3. Install the coversion software in a custom conda environment.
4. Activate the environment 
5. Navigate to the directory containing the Illumina folder and the sample
   sheet `.csv` file. 
6. Start the `bcl2fastq2` program to start the conversion process.

!!! caution
    Remember to check if the `.csv` is protocol A or B! If it is the column i5
    needs to be changed to reverse! If not done, nothing gets mapped!

```bash 
# Move from the login node to a compute node
srun --time=01-01 --mem=32G --ntasks=1 --pty bash -i

# Create the conda environment
conda create -c bih-cubi -n hpc-bcl2fastq2 bcl2fastq2

# Activate the environment
conda activate hpc-bcl2fastq2 

# Check whether the program is available
bcl2fastq --version

# Use `cd` to navigate to the BCL file containing directory e. g.
cd /fast/scratch/groups/ag-drexler/<SOME/FURTHER/FOLDER>
```

The terminal should now point to a folder containing the sample sheet and the
folder containing the Illumia read information, e. g.
`230110_MN01761_0021_A000H3YVT7`. It should look similar to this, with the raw
data in a folder called `data` and an empty `results` folder for saving the
results in later.

```
tree . -L 2
#> ├── data
#> │   ├── 230201 Miniseq.csv
#> │   └── 230201_MN01761_0023_A000H3YVKN
#> │       ├── Config
#> │       ├── Data
#> │       ├── Images
#> │       ├── InstrumentAnalyticsLogs
#> │       ├── InterOp
#> │       ├── Logs
#> │       ├── Recipe
#> │       ├── RTAComplete.txt
#> │       ├── RTAConfiguration.xml
#> │       ├── RTALogs
#> │       ├── RTARead1Complete.txt
#> │       ├── RTARead2Complete.txt
#> │       ├── RTARead3Complete.txt
#> │       ├── RTARead4Complete.txt
#> │       ├── RunCompletionStatus.xml
#> │       ├── RunInfo.xml
#> │       └── RunParameters.xml
#> └── results
```

Use the `bcl2fastq` command to start the conversion. The `nohup` prefix starts
the program as a background process. Using the folder shown above as an
example, run the following command.

```
# Run the program to convert the files from BCL to FASTQ
nohup bcl2fastq \
    --runfolder-dir "data/230201_MN01761_0023_A000H3YVKN" \
    --output-dir "results/" \
    --sample-sheet "data/230201 Miniseq.csv" \
    --no-bgzf-compression
```

Alternatively, the binary of the more recent `BCL Convert` version is stored on
the BIH HPC cluster at
`/fast/work/groups/ag-drexler/software/bcl-convert/bcl-convert-4.1.5-2.el7.x86_64/usr/bin/bcl-convert`
and can be used according to the documentation
[here](https://support-docs.illumina.com/SW/BCL_Convert_v4.0/Content/SW/BCLConvert/BCLConvert.htm).

!!! note "bcl-convert vs. bcl2fastq"
    Depending on the data, using one or the other might be required, because
    the files might be in an old, deprecated structure and not compatible with
    the newer version `bcl-convert`.

## Results

The program should have created a file `nohup.out` containing the output of the
command and the folder `results` should contain the converted files in the
`fastq.gz` format.

```
tree results/ -L 1
#> results/
#> ├── Hanta_S1_L001_R1_001.fastq.gz
#> ├── Hanta_S1_L001_R2_001.fastq.gz
#> ├── HAV_1613_S3_L001_R1_001.fastq.gz
#> ├── HAV_1613_S3_L001_R2_001.fastq.gz
#> ├── HAV_976_S2_L001_R1_001.fastq.gz
#> ├── HAV_976_S2_L001_R2_001.fastq.gz
#> ├── Reports
#> ├── Stats
#> ├── Undetermined_S0_L001_R1_001.fastq.gz
#> └── Undetermined_S0_L001_R2_001.fastq.gz
```

## Further Reading

A longer article outlining the steps can be found [here](https://medium.com/@marija190396/bcl-to-fastq-conversion-e289852823d0).
