# Running nf-core on Alma - Sarek: a real pipeline
`Maintained by: Rachel Alcraft. Last updated: 12th June 2024`

This tutorial covers running a a simple version of sarek with data downloaded from zenodo. This forms the basis of nf-core pipelines that you can build up from with your own data. The important aspect is ensuring you have this working on Alma as the configuration for memory and CPUs is quite specific.

> **Note:** 
> nf-core pipelines are not under our control and may change faster than this documentation. Please do tell us if you have any problems with this tutorial. Please don't assume it is "just you", or the documentation is not supposed to work - we really want it to help everyone and value everyone's input in helping us keep it up-to-date by reporting any problems with it. Send an email to [schelpdesk](mailto:schelpdesk@icr.ac.uk) with the title "Alma Cookbook - Sarek tutorial problem" and it will be filtered to the correct place.

  
> **Important:** 
> Everything in this tutorial is intentional in order to work on Alma so take care with each step ☺️ Note that you need to have an interactive session with at least 10GB and 2 cores to run the workflow interactively.

## Step 1: Log on and activate a mamba session
Note that mamba is the preferred environment, make sure you have followed this tutorial first: [nextflow with mamba](nextflow-envs.md).

```bash
# to alma
ssh username@alma.icr.ac.uk
# interactive session with 10GB memory and 2 cores
srun --pty --mem=10GB -c 2 -t 30:00:00 -p interactive bash    
# navigate to a sensible place
cd /data/scratch/YOUR/PATH/GROUP/username/somewhere/sensible
# activate mamba
mamba activate your-mamba-env
# export the singularity cache variable to a sesnible place
export NXF_SINGULARITY_CACHEDIR=/data/scratch/YOUR/PATH/GROUP/username/.singularity/cache
# check the version of nextflow is as you expect
nf-core --version
```

## Step 2: Run a real pipeline in test
You can run any nf-core pipeline in the same way, just replace testpipeline with the name of the pipeline you want to run. You can find the list of pipelines at [nf-core](https://nf-co.re/pipelines).
For this example we will run the nf-core/sarek pipeline.  We saw a few ways to use an nf-core piepeline in the test, for this we will clone the pipeline.

Starting in the nf-core directory:
```bash
git clone git@github.com:nf-core/sarek.git
cd sarek
nextflow run main.nf -profile test,singularity --outdir my-outdir
```
> **Warning:** 
> The most likely problem with this step will be pulling the singularity images. These can be large and timeout. If you have a problem with this, you can try to pull the images manually, or ask for help. To manually pull you want to use wget instead of singularity as it is quicker, and you navigate to the singularity cache directory (that you specified above) and wget the image from within there. Instructions  on how you turn a singularity error into a wget call are [here](../faqs/faqs.md#5-my-singularity-image-did-not-pull). It could save time if you pull these common problematic images before you begin:
```
# navigate to your singularity cache as above
cd /data/scratch/YOUR/PATH/GROUP/username/.singularity/cache
# pull the 2 images I know to be troublesome:
wget https://depot.galaxyproject.org/singularity/mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb:7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0 -O depot.galaxyproject.org-singularity-mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb-7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0.img

wget https://depot.galaxyproject.org/singularity/gatk4:4.5.0.0--py36hdfd78af_0 -O depot.galaxyproject.org-singularity-gatk4-4.5.0.0--py36hdfd78af_0.img

```

## Step 3: Run a real pipeline with data
[Sarek pipeline](https://nf-co.re/sarek/3.3.2/docs/usage)

We first need to generate some test data (if you don't have some).
Create an inputs directory sarek/inputs.  

[Sample paired reads can be found on zenodo](https://zenodo.org/records/3269404)  
Download these and name them ```T4A_R.fastq``` and ```T4A_F.fastq```

Move the files to the inputs directory within the sarek directory (I just copy and paste with the file system), and navigate there to compress the files, then navigate back:

```bash
cd /data/scratch/YOUR/PATH/GROUP/username/nf-core/sarek/inputs
gzip T4A_R.fastq
gzip T4A_F.fastq
cd ..
```
We need a samplesheet as an input that points to this sample data. The simplest sample data is a pair of fastq files. 
Create a file in the sarek/inputs directory called samplesheet.csv and paste in this:
```
patient,sample,lane,fastq_1,fastq_2
patient1,test_sample,lane_1,inputs/T4A_R.fastq.gz,inputs/T4A_F.fastq.gz
```

Within the sarek directory run the pipeline with the following command, specifying the max_cpus as the ones we have requested an interactive session with (2)
```bash
nextflow run main.nf --input inputs/samplesheet.csv -profile singularity --outdir my-outdir --genome GATK.GRCh38 --max_cpus 2
```

## Step 4: Running sarek on slurm
The reason we are using an HPC cluster is to distribute this work across the cluster's compute nodes. To do this we can specify that we want to use slurm. We also need to pass in some alma specific memory parameters. To do this, create a file in the sarek/conf directory called icr.config, and put inside it this contents:
```
singularity {
  enabled = true
  autoMounts = true
}

process {
    queue="compute"
    executor = "SLURM"
    shell  = ['/bin/bash', '-euo', 'pipefail']
    // memory errors which should be retried. otherwise error out
    errorStrategy = { task.exitStatus in [143,137,104,134,139,140,247] ? 'retry' : 'finish' }
    maxRetries    = 1
    maxErrors     = '-1'
}

executor {
    perCpuMemAllocation = true
}
```

The final section for the execution is important. It tells nextflow to use the memory allocation per CPU that we have requested. This is important as the memory allocation is different on the cluster to the local machine. Notes about this [are here](https://www.nextflow.io/docs/latest/config.html#scope-executor).


You can now run the pipeline using the slurm cluster with this command:

```bash
nextflow run main.nf --input inputs/samplesheet.csv -profile singularity --outdir my-outdir --genome GATK.GRCh38 -c conf/icr.config
```

You will notice that this takes over your session. If you want to monitor the queues you can open another ssh session to alma - you don't need to go an interactive session, you can stay on the login node, and simply type
```
sacct
```
This will give something like this, showing all the running processes on slurm:
```
(base) [ralcraft@login(alma) ~]$ sacct
JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
------------ ---------- ---------- ---------- ---------- ---------- -------- 
13777641           bash interacti+   infotech          2    RUNNING      0:0 
13777641.ex+     extern              infotech          2    RUNNING      0:0 
13777641.0         bash              infotech          2    RUNNING      0:0 
13777644     nf-NFCORE+    compute   infotech         12  COMPLETED      0:0 
13777644.ba+      batch              infotech         12  COMPLETED      0:0 
```

## Step 5: Running sarek on slurm from the master-worker partition
Nextflow has a master thread that is controlling all the other processes. Alma has a special queue for this called the master-worker queue, where long running threads can control their child processs (the compute node has a time limit to stop long running jobs).

To use this queue, kick off the job above using "sbatch" and send it to this master-worker queue.

a) Create a file in the sarek directory called ```sarek-master.sh``` and fill it with this contents:
```
#!/bin/bash
#SBATCH --job-name=nf-username
#SBATCH --output=slurm_out.txt
#SBATCH --partition=master-worker
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=4000

srun nextflow run main.nf --input inputs/samplesheet.csv -profile singularity --outdir my-outdir --genome GATK.GRCh38 -c conf/icr.config
```
Start this job with
```
sbatch sarek-master.sh
```
You can check it has submitted using ```sacct```

The output that previously went to screen can be seen in the file we specified in the batch, in this case ```slurm_out.txt```. This file is recreated each time it is restarted.  

There is also the .nextflow.log - a history of these is kept and they are renamed 1,2,3 etc when a new process is started.  
The work and out-dir are the same as before, and if you find any errors you can match the process up against the directory:






