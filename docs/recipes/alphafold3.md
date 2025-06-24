# Running Alphafold3 on Alma

This guide outlines the steps required to run AlphaFold3 on the Alma HPC. AlphaFold3 is a powerful tool for predicting protein structures, but its setup and usage require careful preparation, including downloading model parameters, configuring input files, and leveraging Singularity. These instructions will walk you through everything from requesting access to model parameters, to preparing your job submission script, to interpreting the output. Please note that AlphaFold3 is available only for non-commercial use, and access to its model parameters must be requested individually.

*IMPORTANT:*

* You need to request the access to Alphafold3 model parameters yourself using the [following form](https://docs.google.com/forms/d/e/1FAIpQLSfWZAgo1aYk0O4MuAXZj8xRQ8DafeFJnldNOnh_13qAx2ceZw/viewform). *You can only use Alphafold3 for non-commercial purposes.* Once the form has been submitted, you will receive an email from Google (after 2-3 days) with a download link. You can then store those parameters on Scratch/RDS. 


A [video tutorial](https://www.youtube.com/watch?v=iIubA9VnutQ&list=PLKk58i7WAwK48DqrcBRTOntUUGAefG-R-) on how to run Alphafold3 following the AlmaCookBook instructions below can be found on our [YouTube channel](https://www.youtube.com/@icrrseteam) (the video is unlisted).

### Clone Alphafold3 (Adapted from [AlaphaFold3 GitHub Repo](https://github.com/google-deepmind/alphafold3))

```
git clone git@github.com:google-deepmind/alphafold3.git
```

### Create input and output directories in the cloned repo

```
mkdir af_output
mkdir af_input
```


### Prep necessary file
Once you have cloned the repo, create a JSON file for your protein sequence. We will be using 2PV7 protein sequence - the example provided on the AlphaFold3 GitHub. Create the JSON file in the `af_input` folder (we will name ours `fold_input.json`):

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

### Create a bash script
You can then create bash script within your AlphaFold3 directory (such as `af3-test.sh`) with specified number of CPUs and GPUs for your needs and run AlphaFold3 specifying necessary flags:

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
    --bind <DATABASES_DIR>:/root/public_databases \
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

`<DATABASES_DIR>` - path to your database

*Important note:* 

* `/data/rds/DIT/SCICOM/SCRSE/shared/singularity/alphafold3.sif` is a path that contains the AlphaFold3 singularity image to be used by everyone in the institute. If you would like to build your own, follow the next section on "What you need to build Alphafold3 Docker image yourself".

There are various flags that you can pass to the run_alphafold.py command, to list them all run python run_alphafold.py --help. Two fundamental flags that control which parts AlphaFold3 will run are:

`--run_data_pipeline` (defaults to true): whether to run the data pipeline, i.e. genetic and template search. This part is CPU-only, time consuming and could be run on a machine without a GPU.

`--run_inference` (defaults to true): whether to run the inference. This part requires a GPU.

### Run sbatch command
After creating the bash script, submit the script with `sbatch` command:

```
sbatch af3-test.sh
```

if you run:

```
squeue -u YOUR_USERNAME
```

You will be able to see your `af3` job running. The job will run for considerable time! (how long depends on your input sequence) 

The outputs will be saved in the `af_output` folder - both the `af3.err` (job runtime errors) and `af3.out` (job runtime outputs) files and a folder named after your protein in the JSON file, in our case that is "2PV7":

![alt text](../assets/af3-output.png)

The output documentation can be found [here](https://github.com/google-deepmind/alphafold3/blob/main/docs/output.md).


# What you need to build Alphafold3 Docker image yourself 

#### 1. Use appropriate hardware.

Docker image only builds on appropriate hardware. We used the following workstation:
CPU: Intel Xeon Gold 5118 CPU 2.30GHz (Sockets: 1, Cores: 12, Threads: 24)
GPU: Nvidia Quadro GV100 (32G)
RAM: 128G

#### 2. Change the original Docker file

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

#### 3. Push Docker imae to remote registry

Docker does not work on Alma. To use the image on Alma, you need to push the Docker image to a remote registry (like DockerHub) and pull a singularity image.
