# Installing R Packages from github

R packages can be installed from github, and these can be troublesome in terms of their dependencies as they are less community tested.
This example shows installatikon of the TwoSampleMR package from github, with an explanation of why some packages are manually installed.  

Note if you are trying to install a package and you get streasm of error messages, the first 
thing to try it to install those packages in mamba with "r-" prepended as below in step 3.

**If you haven't already, follow the [setup mamba instructions](mamba-first.md) to get mamba working.**

1. Log on to an interactive alma session and move somewhere sensible on scratch
```
ssh username@alma.icr.ac.uk
srun --pty --mem=10GB -c 1 -t 30:00:00 -p interactive bash
cd /data/scratch/somewhere/sensible
```

2. Create and activate a virtual environment
```
mamba create --name mamba-TwoSampleMR -c bioconda r-tidyverse r-remotes r-base=4.3
mamba activate mamba-TwoSampleMR
```

3. Manually install some packages in a dependency safe way
```
mamba install r-meta
mamba install r-nloptr
mamba install r-lme4
```

4. Install custom package
```
R -e 'remotes::install_github("MRCIEU/TwoSampleMR")'
```

