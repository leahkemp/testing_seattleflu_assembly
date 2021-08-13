# How to run guide

How to run pipeline on the ESR cluster

- [How to run guide](#how-to-run-guide)
  - [Prerequisites](#prerequisites)
  - [1. Get the pipeline](#1-get-the-pipeline)
  - [2. Create and activate a conda env for the pipeline](#2-create-and-activate-a-conda-env-for-the-pipeline)
  - [3. Setup config file](#3-setup-config-file)
  - [4. Check fastq file naming convention](#4-check-fastq-file-naming-convention)
  - [5. Dryrun](#5-dryrun)
  - [6. Full run deployed to SLURM](#6-full-run-deployed-to-slurm)

## Prerequisites

- [Conda install](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html)
- [Mamba install](https://mamba.readthedocs.io/en/latest/installation.html)

## 1. Get the pipeline

```bash
# Clone repo/pipeline
git clone https://github.com/leahkemp/assembly.git
# Use the branch that I've adapted for ESR
cd assembly
git checkout adapt_for_esr
```

## 2. Create and activate a conda env for the pipeline

```bash
# Only need to run `mamba env create` once
mamba env create -f envs/seattle-flu-environment.yaml
conda activate seattle-flu
```

## 3. Setup config file

Modify config file at `config/config-flu-only.json`

- Set path to directory of fastq files to be analysed (pass to `fastq_directory`)
- Specify if the fastq files have been pooled/lane-merged (pass either "not_pooled" or "pooled" to `fastq_pooling`)
- Set any other parameters

## 4. Check fastq file naming convention

Expected filename format: `318375_R1.fastq.gz` where `318375` is the NWGC sample ID

## 5. Dryrun

Make sure you're in the pipeline directory, for example:

```bash
cd /NGS/scratch/KSCBIOM/HumanGenomics/assembly/
```

Start a dryrun to check everything is in order before doing a full run of the pipeline

```bash
snakemake --configfile config/config-flu-only.json -k -n
```

If you get no jobs that will be run such as...

```bash
localrule all:
    jobid: 0
    resources: tmpdir=/tmp

Job stats:
job      count    min threads    max threads
-----  -------  -------------  -------------
all          1              1              1
total        1              1              1

This was a dry-run (flag -n). The order of jobs does not reflect the order of execution.
```

...check the path to the fastq directory in the config file

Should get something like:

```bash
Job stats:
job                       count    min threads    max threads
----------------------  -------  -------------  -------------
aggregate                    16              1              1
all                           1              1              1
bamstats                     16              1              1
index_reference_genome        4              1              1
map                          16              1              1
mapped_reads                 16              1              1
post_trim_fastqc              4              1              1
sort                         16              1              1
trim_fastqs                   4              1              1
total                        93              1              1

This was a dry-run (flag -n). The order of jobs does not reflect the order of execution.
```

## 6. Full run deployed to SLURM

`-j20` sets the maximum number of threads to use at one time to 20

- Pass your username to the `-A` flag (for example `-A lkemp`)

```bash
snakemake -j 20 -k -w 60 \
--configfile config/config-flu-only.json \
--cluster-config config/cluster.json \
--cluster "sbatch -A lkemp -p prod --nodes=1 --tasks=1 --mem={cluster.memory} --cpus-per-task={cluster.cores} --time={cluster.time} -o all_output.out"
```
