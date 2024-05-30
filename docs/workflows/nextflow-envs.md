# Nextflow in environments

The default nextflow on Alma is only version 20 and this is unlikely to be sufficient for recent pipelines.  
Create an environment for your nextflow pipelines using conda or mamba, or a python virtual environment.  

If you load the mamba environment, you will have the correct java version for nextflow. If you use a python virtual environment, you will need to load the correct java version yourself.  


# Using mamba to create a nextflow environment

```bash
mamba create --name mamba_nf nextflow
mamba activate mamba_nf
nextflow -v
```

# Using conda to create a nextflow environment

```bash
conda create --name conda_nf nextflow
conda activate conda_nf
nextflow -v
```

# Using a python virtual environment to create a nextflow environment

```bash
python3 -m venv venv_nf
source venv_nf/bin/activate
pip install nextflow
module load java/jdk15.0.1
nextflow -v
```
