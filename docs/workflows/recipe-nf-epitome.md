# Epi2me Labs Nextflow Pipeline

This document walks you through running [Oxford Nanopore's](https://community.nanoporetech.com/docs/analyse/epi2me-workflows) test pipeline using Nextflow.  The pipeline can be run directly from [github](https://github.com/epi2me-labs), and in this example we are looking at the [basecalling workflow](https://github.com/epi2me-labs/wf-basecalling).

It is worth noting that this example is using the GPUs on Alma and is therefore following a recipe in order for this to work specifically on Alma. 
The solution given here is to use a dual-sbatch pattern to run nextflow from a GPU.

In this example mamba and conda are interchangeable, so stick with 1 or the other. I use mamba. If you need any help setting up mamba or conda, or you are not sure if you have it activated on Alma, please see the [mamba installation guide](../conda/mamba-first.md).

All the work below is done from the command-line remaining in your chosen directory on scratch.  

### 1. Work on an interactive node in a suitable directory you have created.

```bash
ssh username@alma.icr.ac.uk
srun --pty --mem=8GB -c 1 -t 30:00:00 -p interactive bash
cd /data/scratch/your/path/
```

### 2. Create and activate a mamba environment

```bash
mamba create --name epi2me-labs -c bioconda -c conda-forge nextflow pytorch cuda
mamba activate epi2me-labs
nextflow -v # check that nextflow is installed
```

### 3. Pull the nextflow pipeline from github

```bash
nextflow pull epi2me-labs/wf-basecalling
```

### 4. Setup the test data
    
```bash
wget https://ont-exd-int-s3-euwst1-epi2me-labs.s3.amazonaws.com/wf-basecalling/wf-basecalling-demo.tar.gz
tar -xzvf wf-basecalling-demo.tar.gz
```
### 5. Create the first of 2 batch files: the nextflow call
This script is the sbatch call that will call nextflow from the gpu.  
Copy the contents below into a file that you name *nextflow-basecalling.sh* in your working directory.  

```bash
#!/usr/bin/env bash

source ~/.bashrc

mamba activate epi2me-labs
module load cuda/11.1

nextflow run epi2me-labs/wf-basecalling \
-profile singularity \
--input wf-basecalling-demo/input \
--ref wf-basecalling-demo/GCA_000001405.15_GRCh38_no_alt_analysis_set.fasta \
--dorado_ext pod5 \
--out_dir output \
--basecaller_cfg dna_r10.4.1_e8.2_400bps_hac@v4.1.0 \
--remora_cfg "dna_r10.4.1_e8.2_400bps_hac@v4.1.0_5mCG_5hmCG@v2"
--cuda_device cuda:all

mamba deactivate
```

### 6. Create the second of 2 batch files: the calling script
This script will submit the work to a gpu.  
Copy the contents below into a file that you name *epi2me-basecalling.sh* in your working directory.  

```bash
#!/usr/bin/env bash
#SBATCH --job-name=epi2me
#SBATCH --output=epi2me.out
#SBATCH --error=epi2me.err
#SBATCH --partition=gpu
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --cpus-per-task=12
#SBATCH --mem-per-cpu=4096
#SBATCH --gres=gpu:2

module load cuda/11.1
srun nextflow-basecalling.sh
```

### 7. Make sure you have the correct permissions on the files to execute

```bash
chmod +x epi2me-basecalling.sh
chmod +x nextflow-basecalling.sh
```

### 8. Submit the job

```bash
sbatch epi2me-basecalling.sh
```

### 9. Check the status of the job
You can monitor the queues with the following command:

```bash
squeue -u username
```

You can monitor the progress of the job by looking at the output and error files:

```bash
cat epi2me.out
cat epi2me.err
cat .nextflow.log
```

The epi2me.out file will give you a good idea of the progress of the job. A successful completion of this test job will look something like this:

```bash
[fc/77f55a] wf_dorado:dorado (3)           | 3 of 3 ✔
[e4/727b8d] wf_dorado:make_mmi             | 1 of 1 ✔
[df/b1bb31] wf_…ado:align_and_qsFilter (3) | 3 of 3 ✔
[90/ab4420] wf_dorado:merge_pass_calls     | 1 of 1 ✔
[e9/edf56a] wf_dorado:merge_fail_calls     | 1 of 1 ✔
[a0/359e6d] getVersions                    | 1 of 1 ✔
[07/05f7fa] getParams                      | 1 of 1 ✔
[49/9a87e1] cram_cache                     | 1 of 1 ✔
[5b/358c65] bamstats (3)                   | 3 of 3 ✔
[81/3d7644] progressive_stats (3)          | 3 of 3 ✔
[c2/a4b052] makeReport (3)                 | 3 of 3 ✔
[0a/fe7211] output_last                    | 1 of 1 ✔
[79/0e009d] output_stream (8)              | 9 of 9 ✔
Waiting for file transfers to complete (2 files)
Completed at: 31-May-2024 12:38:41
Duration    : 6m 41s
CPU hours   : 1.0
Succeeded   : 31
```
