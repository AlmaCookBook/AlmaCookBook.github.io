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

### ensembl-vep
This is a package for variant effect prediction [found on github](https://github.com/Ensembl/ensembl-vep).  
It is most commonly installed from [github with perl](https://www.ensembl.org/info/docs/tools/vep/script/index.html) but can be [installed with conda](https://bioconda.github.io/recipes/ensembl-vep/README.html).  

When installed with conda, the package is called `ensembl-vep`.  The [plugins are aliased](https://www.biostars.org/p/9561573/) as `vep_install`, be;low shows the creation of a conda environment and the [installation of the human cache](https://stackoverflow.com/questions/70801077/how-to-run-ensembl-vep-in-conda).  

```bash
mamba create --name bio-perl-vep -c conda-forge -c bioconda -c defaults perl-bioperl=1.7.8 ensembl-vep
mamba activate bio-perl-vep
vep -help
vep_install -help
```
Example usage:  
```bash
vep_install -a cf -s homo_sapiens -y GRCh38 -c ./ —CONVERT

vep --cache --dir_cache "./" \
   -i "./C1D_filtered_SOPRANO.vcf" \
   -o "./vep_output.txt" \
   --offline
```
You must have the cache and dir-cache flags specified, and the human data needs to be in the dir-cache. There is no online access on Alma.  
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

