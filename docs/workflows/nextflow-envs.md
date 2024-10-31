# Nextflow in environments

The default Nextflow on Alma is only version 20 and this is unlikely to be sufficient for recent pipelines.  
Create an environment for your Nextflow pipelines using conda or mamba, or a python virtual environment.  

If you load the mamba environment, you will have the correct java version for Nextflow. If you use a python virtual environment, you will need to load the correct java version yourself.  

When using mamba or conda (they are interchangeable, be consistent - mamba is preferred), 
make sure you have first followed the recommended way to ensure they are correctly initisalised on Alma: 
[mamba installation guide](../conda/mamba-first.md).

The most recent Nextflow version in mamba as of May 2024 is `nextflow version 24.04.2.5914`  
    
    
### Using mamba to create a Nextflow environment

```bash
mamba create --name mamba_nf -c bioconda nextflow=24.04.4-0
mamba activate mamba_nf
nextflow -v
```

### Using conda to create a Nextflow environment

```bash
conda create --name conda_nf -c bioconda nextflow
conda activate conda_nf
nextflow -v
```

#### Using a python virtual environment to create a Nextflow environment

```bash
python3 -m venv venv_nf
source venv_nf/bin/activate
pip install nextflow
module load java/jdk15.0.1
nextflow -v
```
