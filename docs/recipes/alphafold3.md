# Running Alphafold3 on Alma

This guide outlines the steps required to run AlphaFold3 on the Alma HPC. AlphaFold3 is a powerful tool for predicting protein structures, but its setup and usage require careful preparation, including downloading model parameters, configuring input files, and leveraging Singularity. These instructions will walk you through everything from requesting access to model parameters, to preparing your job submission script, to interpreting the output. Please note that AlphaFold3 is available only for non-commercial use, and access to its model parameters must be requested individually.

*IMPORTANT:*

* You need to request the access to Alphafold3 model parameters yourself using the [following form](https://docs.google.com/forms/d/e/1FAIpQLSfWZAgo1aYk0O4MuAXZj8xRQ8DafeFJnldNOnh_13qAx2ceZw/viewform). *You can only use Alphafold3 for non-commercial purposes.* Once the form has been submitted, you will receive an email from Google (after 2-3 days) with a download link. You can then store those parameters on Scratch/RDS. 

* Please make sure you read legal documents on the [outputs terms of use](https://github.com/google-deepmind/alphafold3/blob/main/OUTPUT_TERMS_OF_USE.md), [weights terms of use](https://github.com/google-deepmind/alphafold3/blob/main/WEIGHTS_TERMS_OF_USE.md) and [weights prohibited use policy](https://github.com/google-deepmind/alphafold3/blob/main/WEIGHTS_PROHIBITED_USE_POLICY.md).

* Alphafold3 only works on GPUs, therefore, you will be charged GPU rates, which is currently 240p/(GPU core)-hour. 


A [video tutorial](https://www.youtube.com/watch?v=iIubA9VnutQ&list=PLKk58i7WAwK48DqrcBRTOntUUGAefG-R-) on how to run Alphafold3 following the AlmaCookBook instructions below can be found on our [YouTube channel](https://www.youtube.com/@icrrseteam) (the video is unlisted). 

All material is adapted from [AlaphaFold3 GitHub Repo](https://github.com/google-deepmind/alphafold3).

### Create a dedicated directory to run Alphafold3 on Scratch/RDS and `cd` into it

```
mkdir alphafold3
cd alphafold3
```

### Create input and output directories in the `alphafold3` directory

```
mkdir af_output
mkdir af_input
```


### Prep necessary files
To prepare necessary files you can use your favourite text editor. For example using `nano`:

```
nano <FILE_NAME>.py # creating a python script
```

#### JSON file
Once you have cloned the repo, create a JSON file for your protein sequence. We will be using 2PV7 protein sequence - the example provided on the AlphaFold3 GitHub. Create the JSON file in the `af_input` folder (such as `fold_input.json`).

Example:

Create `fold_input.json` file

```
nano fold_input.json
```

Copy the following JSON object in the file.
```
{
  "name": "2PV7",
  "sequences": [
    {
      "protein": {
        "id": ["A", "B"],
        "sequence": "GMRESYANENQFGFKTINSDIHKIVIVGGYGKLGGLFARYLRASGYPISILDREDWAVAESILANADVVIVSVPINLTLETIERLKPYLTENMLLADLTSVKREPLAKMLEVHTGAVLGLHPMFGADIASMAKQVVVRCDGRFPERYEWLLEQIQIWGAKIYQTNATEHDHNMTYIQALRHFSTFANGLHLSKQPINLANLLALSSPIYRLELAMIGRLFAQDAELYADIIMDKSENLAVIETLKQTYDEALTFFENNDRQGFIDAFHKVRDWFGDYSEQFLKESRQLLQQANDLKQG"
      }
    }
  ],
  "modelSeeds": [1],
  "dialect": "alphafold3",
  "version": 1
}
```

Input documentation can be found [here](https://github.com/google-deepmind/alphafold3/blob/main/docs/input.md).

#### `run_alphafold.py` script

Create a `run_alphafold.py` script in your `alphafold3` directory and paste the code provided in the following [link](https://raw.githubusercontent.com/google-deepmind/alphafold3/refs/heads/main/run_alphafold.py).

Example:

Create `run_alphafold.py` file

```
nano run_alphafold.py
```

Then paste the code in the link (its very long, so we won't display all of it here):

```
# Copyright 2024 DeepMind Technologies Limited
#
# AlphaFold 3 source code is licensed under CC BY-NC-SA 4.0. To view a copy of
# this license, visit https://creativecommons.org/licenses/by-nc-sa/4.0/
#
# To request access to the AlphaFold 3 model parameters, follow the process set
# out at https://github.com/google-deepmind/alphafold3. You may only use these
# if received directly from Google. Use is subject to terms of use available at
# https://github.com/google-deepmind/alphafold3/blob/main/WEIGHTS_TERMS_OF_USE.md

"""AlphaFold 3 structure prediction script.

AlphaFold 3 source code is licensed under CC BY-NC-SA 4.0. To view a copy of
this license, visit https://creativecommons.org/licenses/by-nc-sa/4.0/

cont...
```

### Create a bash script
You can then create bash script in your `alphafold3` directory (such as `af3-test.sh`) with specified number of CPUs and GPUs for your needs and run AlphaFold3 specifying necessary flags.

Example:
Create `af3-test.sh` file

```
nano af3-test.sh
```

Copy the following script

```
#!/bin/bash
#SBATCH --job-name=af3
#SBATCH --partition=gpu
#SBATCH --output=af_output/af3.out
#SBATCH --error=af_output/af3.err
#SBATCH --time=00:20:00
#SBATCH --cpus-per-task=20 
#SBATCH --gres=gpu:1

singularity exec \
    --nv \
    --bind $HOME/af_input:/root/af_input \
    --bind $HOME/af_output:/root/af_output \
    --bind <MODEL_PARAMETERS_DIR>:/root/models \
    --bind /data/reference-data/alphafold_db:/root/public_databases \
    /data/rds/DIT/SCICOM/SCRSE/shared/singularity/alphafold3.sif \
    python run_alphafold.py \
    --flash_attention_implementation=xla \
    --json_path=/root/af_input/fold_input.json \
    --model_dir=/root/models \
    --db_dir=/root/public_databases \
    --output_dir=/root/af_output
```

Where:

`$HOME` - is the path to the directory where you are running AlphaFold3 from 

`<MODEL_PARAMETERS_DIR>` - path to your downloaded model parameters

`/data/reference-data/alphafold_db` - path to protein database in ICR's shared data folder

*IMPORTANT:* 

* `/data/rds/DIT/SCICOM/SCRSE/shared/singularity/alphafold3.sif` is a path that contains the AlphaFold3 singularity image to be used by everyone in the institute. If you would like to build one for your own machine, follow the next section on "What you need to build Alphafold3 Docker image yourself - Important Notes".

* Depending on the length and complexity of your protein, you need to specify an appropriate number of GPUs, CPUs and time for your job to complete. 

* `--flash_attention_implementation=xla` flag is very important to run the image successfully on Alma. Do not remove it.

There are various flags that you can pass to the run_alphafold.py command, to list them all run python run_alphafold.py --help. Two fundamental flags that control which parts AlphaFold3 will run are:

`--run_data_pipeline` (defaults to true): whether to run the data pipeline, i.e. genetic and template search. This part is CPU-only, time-consuming and could be run on a machine without a GPU.

`--run_inference` (defaults to true): whether to run the inference. This part requires a GPU.

### Run `sbatch` command
After creating the bash script, submit the script with `sbatch` command:

```
sbatch af3-test.sh
```

if you run:

```
squeue -u <YOUR_USERNAME>
```

You will be able to see your `af3` job running. The job will run for considerable time! (depending on your input sequence) 

The outputs will be saved in the `af_output` folder - both the `af3.err` (job errors) and `af3.out` (job outputs) files and a folder named after your protein in the JSON file, in our case that is "2PV7":

![alt text](../assets/af3-output.png)

The output documentation can be found [here](https://github.com/google-deepmind/alphafold3/blob/main/docs/output.md).


# What you need to build Alphafold3 Docker image yourself - Important Notes

*IMPORTANT:*

* This is applicable if you want to run Alphafild3 on your own machine. Please use the shared singularity image if you are running Alphafold3 on Alma. 

#### 1. Use appropriate hardware.

Docker image only builds on appropriate hardware. We used the following workstation to build one for Alma:

CPU: Intel Xeon Gold 5118 CPU 2.30GHz (Sockets: 1, Cores: 12, Threads: 24)

GPU: Nvidia Quadro GV100 (32G)

RAM: 128G

#### 2. [OPTIONAL] Change the original Docker file if using CUDA Capability 7.x GPUs

CUDA Compute Capability 7.x GPUs have limited numerical accuracy and performance (more [here](https://github.com/google-deepmind/alphafold3/blob/main/docs/known_issues.md)). If you are using one of those on your system (Alma does), you need to change a few lines in the Dockerfile. 

The original Dockerfile has the following lines near the end:

```
# To work around a known XLA issue causing the compilation time to greatly
# increase, the following environment variable setting XLA flags must be enabled
# when running AlphaFold 3. Note that if using CUDA capability 7 GPUs, it is
# necessary to set the following XLA_FLAGS value instead:
# ENV XLA_FLAGS="--xla_disable_hlo_passes=custom-kernel-fusion-rewriter"
# (no need to disable gemm in that case as it is not supported for such GPU).
ENV XLA_FLAGS="--xla_gpu_enable_triton_gemm=false"
# Memory settings used for folding up to 5,120 tokens on A100 80 GB.
ENV XLA_PYTHON_CLIENT_PREALLOCATE=true
ENV XLA_CLIENT_MEM_FRACTION=0.95
```

Since Alma has CUDA capability 7 GPU card, you need to:

```
# Uncomment this line
ENV XLA_FLAGS="--xla_disable_hlo_passes=custom-kernel-fusion-rewriter"
# Comment out this line
# ENV XLA_FLAGS="--xla_gpu_enable_triton_gemm=false"
```

#### 3. Push Docker image to remote registry

Docker does not work on Alma. If you are using a system that does not have Docker, you can use Singularity (which is also what Alma has available). If that applies to you, you need to push the Docker image to a remote registry (like DockerHub) and pull a Singularity image.
