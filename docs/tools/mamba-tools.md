# Using Scientific Tools with Mamba and Conda

This assumes that you have already followed the [using mamba for the first time](../conda/mamba-first.md) tutorial on this site.  

Mamba environments manage dependencies and packages for you, and are the preferred way to install R packages.  
Most R packages can be installed from mamba by prepending "r-" eg "r-tidyverse" etc. 
Only where the package does not exist is it recommended to use `package.install` in R.  

*Note* - all of these installations assume you have logged on to an interactive session on Alma.  

---  
### Nextflow (latest version)
See further nextflow instructions [here](../containers/nextflow-envs.md).  

```bash
mamba create --name mamba_nf -c bioconda nextflow
mamba activate mamba_nf
nextflow -v
```

---  

### MRCIEU/TwoSampleMR
This is a package for Mendelian Randomization analysis [found on github](https://github.com/MRCIEU/TwoSampleMR).  

- Create and activate a virtual environment  
```bash
mamba create --name mamba-TwoSampleMR -c bioconda samtools bcftools r-tidyverse r-remotes r-base=4.3
mamba create --name mamba-TwoSampleMR -c bioconda r-tidyverse r-remotes r-base=4.3
mamba activate mamba-TwoSampleMR
```
- Manually install some packages in a dependency safe way  
```bash
mamba install r-meta
mamba install r-nloptr
mamba install r-lme4
```
- Install custom package  
```bash
R -e 'remotes::install_github("MRCIEU/TwoSampleMR")'
```

---  

