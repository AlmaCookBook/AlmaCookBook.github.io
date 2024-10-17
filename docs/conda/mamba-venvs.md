# Creating Mamba Environments

First get an interactive session:
```
ssh username@alma.icr.ac.uk
srun --pty -t 12:00:00 -p interactive bash
```

This assumes that you have already followed the [using mamba for the first time](mamba-first.md) tutorial on this site.  
Conda environments can be shared amongst users, and for yourself you can share them between different applications, e.g., all your nextflow pipelines can use the same environment.

Mamba environments manage dependencies and packages for you, and are the preferred way to install R packages. 
Most R packages can be installed from mamba wiby prepending "r-" eg "r-tidyverse" etc. 
Only where the package does not exist is it recommended to use package.install in R.

Using conda environments enables you to use more up-to-date versions of software than are available in the standard modules.  

It also facilitates sharing, re-use, and reproducibility of your work.

N.b. after creating a new environment, you can activate it with the command:  
```bash
mamba activate myenv
```
To deactivate it use:  
```bash
mamba deactivate
```

## Creating environments

### 1. In the standard way  
```bash
mamba create --name myenv  
```
This will be located in the conda home directory set up in your path.  To find the directories used for environments call:  
```bash
conda config --show envs_dirs
```
You will see any shared (read only) environments you have access too along with your own personal environments.  
**Note** because the default is your home directory which is does not have much space, you may want to move the default to SCRATCH.  

Once activated anything you install will be installed in the active environment, including any python pip installs. 
To install packages into the environment, eg numpy:
```bash
mamba install numpy
```

### 2. In a specific location  

```bash
mamba create --prefix /path/to/env myenv
```

### 3. With a specific version of Python  

```bash
mamba create --name my-env python=3.8
```

### 4. With a specific version of a package  

```bash
mamba create --name my-env package=1.0
```

### 5.  With a specific version of a package and Python  

```bash
mamba create --name my-env python=3.8 package=1.0
```
### 6. With a specific version of R

```bash
mamba create --name my-env /path/to/env r-base=4.1
```

## Some common environments using channels

Mamba uses channels to search for packages. A package could be present in multiple channels. 
A channel is specified using the -c argument.
A channel is an independent and isolated repo structure that is used to classify and administrate more easily a package server. It comes with a repodata.json file that is the index of all available packages.

Examples of existing channels:
1. bioconda
2. conda-forge
3. defaults 

### 1. From the bioconda channel: samtools and bcftools packages

```bash
mamba create --name my-env -c bioconda samtools bcftools r-base=4.3
```

### 2. From the bioconda channel: nextflow and nf-core tools

```bash
mamba create --name my-env -c bioconda nextflow nf-core
```

### 3. From the conda-forge channel: R with tidyverse

```bash
mamba create --name my-env -c conda-forge r-tidyverse
```

### 4. From the conda-forge channel: Python with pandas and numpy

```bash
mamba create --name my-env -c conda-forge python=3.8 pandas numpy
```

## Sharing environments
You can save your environment to a file and share it with others.  
```bash
mamba env export --name my-env > my-env.yml
```
This will create a file called my-env.yml in your current directory.  
You can load this environment on another machine with the command:  
```bash
mamba env create --file my-env.yml
```

## Removing environments
To remove an environment, use the command:  
```bash
mamba env remove --name my-env
```
This will remove the environment from your system.  

## Trouble shooting  

### 1.  conda/mamba environment is broken  
If you have a broken conda/mamba environment, you can try to fix it by removing the environment and creating it again. You can do this with the commands (conda or mamba):
```bash
conda env remove --name myenv
conda create --name myenv
```

However sometimes the environments are so badly broken you cannot use an conda commands at all and get unhandled or unknown excpetions. 
In these rare circumstances you can totally refresh your installation by:
 - Renaming the .conda directory in your home directory
 - Renaming the .condarc file in your home directory
 - Remving the conda init section in your .bashrc file which means deleting this entire section
```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
Delete all this stuff too, as well as the >>> conda initialize >>> line
and the <<< conda initialize <<< line
# <<< conda initialize <<<
```
 - Go though the [init process](../first_steps.md) to set up conda and your environment again.

### 2. There is no space left!
If you are running out of space in your home directory, you can move the conda environment to SCRATCH. 
You can do this by creating a simlink to the SCRATCH directory.  
 - Copy your home/.conda directory to SCRATCH somewhere you will remember it is!
 - Remove the .conda directory in your home directory
  - Create a simlink to the SCRATCH directory
```bash
cp -r ~/.conda /data/scratch/DCO/DIGOPS/SCIENCOM/ralcraft/.conda_simlink
rm -r -f ~/.conda
ln -s /data/scratch/DCO/DIGOPS/SCIENCOM/ralcraft/.conda_simlink ~/.conda
```

## Further reading
- [Mamba documentation](https://mamba.readthedocs.io/en/latest/)







  
