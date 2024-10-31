# Using R Scripts on Alma - Environments

Creating an environment with your installations will enable you to use R on Alma in a reproducible manner. The environment can be shared with colleagues, help desk, and collaborators to ensure that the same environment is used for the same results. It also means that in the future you can recreate the environment to run the same notebooks.

**If you haven't already, follow the [setup mamba instructions](mamba-first.md) to get mamba working.**

1. First get an interactive session:
```
ssh username@alma.icr.ac.uk
srun --pty -t 12:00:00 -p interactive bash
mamba --version
```

2. Follow the steps for creating a new environment in the [mamba documentation](python-scripts.md) and activate it.
Create a mamba environment, just like with python, though there are a few modules we know we are likely to want with R. 
```shell
mamba create --name my-mamba-r -c conda-forge -c bioconda -c r -c defaults r-base=4.3
mamba activate my-mamba-r
```

3. Check you have the environment you expect
```shell
R --version
```

4. Load CMake
You will need to load the CMake module to install some R packages that need to be installed from source.
```shell
module load CMake
```

4. Installing packages
It is preferable to use mamba to install R packages like ggplot2 etc. In general, if there is a package that you could install with install(packages), you can install it with mamba install with an "r-" in the front - not always, but it is the best way to try.
```
mamba install r-ggplot2
```

5. Packages that do not have an "r-" module
Can be loaded in an R script in the usual way, or form the command line like:
```shell
R -e "install.packages('ggplot2', repos='https://cloud.r-project.org')"
```

6. Export and import the environment
```shell
# exporting the environment
mamba env export --name my-mamba-r > my-mamba-r.yml
```

7. Deactivate the environment
```shell
mamba deactivate
```

8. import the environment
```shell
mamba env create --file my-mamba-r.yml
```

