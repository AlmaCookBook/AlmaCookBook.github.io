# Nextflow vs slurm

This article compares a nextflow workflow with a slurm workflow using a very simple so-called embarrassingly parallel workflow as an example.

### This article covers:  
- how to convert an existing slurm workflow to nextflow
- a simple introduction to nextflow
- a simple help to get nextflow working on alma
- a simple introduction to running a slurm job on alma

At the end of this you will have a simple dependent array job in slurm running on alma, alongside a nextflow workflow that gives the same answer. Both workflows will call the same python scripts that are doing the real work.

### Why use nextflow?
There are pros and cons to any method, and there are other workflow methods being used at the ICR. Some of the advantages of nextflow are:

- adoption by the bioinformatics community to produce a catalogue of open-source, maintained pipelines at nf-core 
- you can create re-usable modules
- it is platform independent, you can run on slurm or locally with very little change
- checkpointing is built in, so if something fails it can pick up where it left off
- the dependencies are sophisticated so you can build whatever you need
- there is seemless integration with docker and singularity

### What does this workflow do?
The workflow is based on input-output of files and directories, consistent with the way many bioinformatics applications work.  This example will do the following:

- Using a python script, create files for the numbers 1:n - creating a file for each
- Once this is complete, seperate processes take each of these files and square them, unecessarily using numpy and sleeping for a user defined time to demo
- Once that is complete, all the results are added up and published to a results directory
- Links will be given for further information at various steps to keep the document simple.

### Tutorial
1. Ensure you have access to alma
If you are a new user to alma you will need to [request access](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/Pages/Getting-Started.aspx#anchor3).

See above also for a reminder of how to access alma, or some [documents on slurm here](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/userguides/Pages/Use-Cases.aspx).

2. Check you anaconda environment
You want to be running an updated anaconda environment which will ensure a recent python. If you have not already done this, then follow [these instructions here](../conda/mamba-first.md). 

```
$ python3 --version
Python 3.8.3
```

3. Log on to an interactive/compute node
Once at the alma login prompt, access a compute node:

```
$ ssh username@alma.icr.ac.uk
$ srun --pty -t 12:00:00 -p interactive bash
```
The interactive nodes are required for running nextflow as singularity is not installed on the login nodes. You do not need to load singularity, it is loaded by default. In this example we will not be using singularity, look at the nf-core training page  for more details.

4. Navigate to a good location
Navigate to a good location where you will pull a github repo. Create a directory that you might pull in a few repos for further training. Example shown for the scratch data in Scientific Computing.

```
$ cd /data/scratch/a/good/location/
```

5a. Running this session for the first time
Clone the github repo and then navigate to that directory. Then, you will need to make sure that python and nextflow are installed in a virtual environment.

```
$ git clone git@github.com:ICR-RSE-Group/nextflow-toy.git
$ cd nextflow-toy
$ cd toy_submission_
$ python3 -m venv venv
$ source venv/bin/activate
$ chmod +x squaring_slurm.sh
$ chmod +x squaring_nf.sh
$ pip install --upgrade pip
$ pip install nextflow
$ pip install numpy

5b. Returning to this session
Activate the python environment. Navigate to the directory, and then:

```
$ cd nextflow-toy
$ cd toy-workflow
$ source venv/bin/activate
```
If you need to pull the latest version of the tutorial (and lose your changes):
```
$ git checkout -- .
$ git pull
$ chmod +x squaring_slurm.sh
$ chmod +x squaring_nf.sh
```

6. Check everything is loaded and the right version
Load java and check python and nextflow to be sure they are loaded
```
$ module load java/jdk15.0.1
$ nextflow -v
$ python --version
```
You might want to open the github repo so you can browse the code as you go along.

7. The old way - a slurm workflow
Submit each 1 by 1

```
$ sbatch squaring_1_split.sh
$ sbatch --dependency=afterok:01234 bin/squaring_2_square.sh
$ sbatch --dependency=afterok:56789 bin/squaring_3_gather.sh
```
Or submit as a workflow
```
$ ./squaring_slurm.sh
```
1-by-1 requires you to type in the job dependency each time, but is simpler if you are testing 1 step. The batch submits them as dependent batches.

8. How do you know if slurm worked?
When it has finished there will be a results file in the results/slurm_total.txt Each step is saving its file in work_slurm so you can see the intermediate results there.

9. Monitoring and managing the slurm queues
To check if your jobs have been successfully submitted and to get an idea of their status you can monitor the slurm queues with:
`$ squeue -u username`
You can cancel them using:
`$ scancel 12345`

10. Running this same workflow with nextflow
Run it locally in a single process.

```
$ nextflow run squaring_nf.nf
# or
$ nextflow run squaring_nf.nf -resume
```

The resume flag is where the checkpointing is handled - the first time there is nothing to resume, but after that it will pick up where it left off. Take off the resume flag if you want it to recalculate.

The sleep flag can be changed from the command line easily:

```
$ nextflow run squaring_nf.nf --sleep 20
```
You will see in the workfolder the command.sh file now passes the parameter as 20.

The configuration of nextflow batches  is complex, with precendence being given to command-line parameters first.

11. Has it worked?
Each step of the calculation creates a new file: split creates a file for each "n" which you will find in the work directories. Every nextflow job has its own folder in "work" where it has all the data it needs to run.

Finally the data is published to results/nextflow_total.txt

12. Running nextflow on slurm
You can run the entire thing on slurm with hardly any change.

```
$ nextflow run squaring_nf.nf -process.executor slurm
```
Simply specify the process executor to be slurm and it will magically be distributed across nodes.

10. Slurming the nextflow controller
You may have noticed that while running the nextflow workflow, you had to be running the compute node actively. The final step is to submit the nextflow processor to slurm so the master-worker controller is managing the workflow. Some info on this here: master-worker nextflow 

For this, a batch file is created to submit nextflow via sbatch.

$ sbatch squaring_nf.sh
You know that it has worked when the file results/nextflow_total.txt is updated. Slurm has log files too, and this has been specified in the header of the slurm batch in the parameter --output

```
#!/bin/bash
#SBATCH --job-name=nf
#SBATCH --output=work_slurm/nf.txt
#SBATCH --partition=master-worker
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=1000

srun nextflow run squaring_nf.nf -process.executor slurm
```

Look in work_slurm/nf.txt and you will see that it contains what was in the command prompt before, it will keep updating with the queue status until complete where it will log some basic stats.

NOTE
It is safe to empty the work folder every time you rerun nextflow if you are interested in analysing what is going on. I recommend spending sometime looking at the contents of the work directry and how it matches up with the scripts.

The workflow is used for "resume" though so it is not safe if you want to re-run with the resume flag.

Continue with your nextflow learning
You can continue your nextflow learning at nextflow, with their tutorial [basic training](https://training.nextflow.io/basic_training/intro/#in-practice).

