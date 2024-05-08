# First Steps to using Alma

Before you start running applications on Alma, you need to set up some basic things to make life easier 
and to ensure that the majority of further help tutorials are accurate for your environment.

If you don't think any of this is going to be necessary, feel free to skip it, but it is recommended.  

1. **Github account**  
If you don't have a github account, you can create one [here](https://docs.github.com/en/get-started/onboarding/getting-started-with-your-github-account). 
Use your icr email address for the github account, and once you have one raise a ticket with the 
[scientific computing helpdesk](mailto:schelpdesk@icr.ac.uk) and ask to be added to the ICR github organisation.


2. **Git with ssh keys**  
Many applications are integrated with git and having it set up correctly from the outset will save problems down the line. 
You want to have it set up with ssh access from Alma to github. 
    - Check first if you [have ssh keys set up](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys).  
    - Otherwise [generate a new one](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).
    - Then [add the ssh key to github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

3. **The Alma fileshare**  
You can use file explorer on windows or the mac equivalent to access files in your scratch area of alma from:  
\\alma-fs\SCRATCH  
This allows you to move files between your local machine and Alma and do editting of files directly on Alma. If you prefer (or need to access home), there are various browser applications such as WinSCP for the file system.

3. **Conda and Mamba**  
You should initialise mamba and conda in your shell profile. This will make sure that the conda and mamba commands are available in your shell.  
Instructions are here: [Conda and Mamba Initialise](conda/mamba-first.md)

4. **Python and R OnDemand**  
Make sure you can use the ondemand applications you will require here: [Alma Open OnDemand](https://alma-ondemand.icr.ac.uk).  
RStudio uses the Alma installation of R and does not allow for environments (unlike R in scripts which can be used in a mamba environment) 
but [jupyter notebooks can be used with a conda environment](conda/python-ondemand.md).  

5. **Nextflow**  
Alma has a shared installation of Nextflow, but you can also access nextflow through python virtual environments or conda environments.
Instructions are here: [Nextflow](workflows/nextflow-envs.md)  
nf-core pipelines are also available on Alma, the complexity is in the way the pipelines are run on the slurm executor.
Instructions are here for [getting started](workflows/nf-core-1.md) and [running an nf-core pipeline](workflows/nf-core-2.md).

6. **Docker and singularity**  
An alternative to conda environments is creating a docker image and running it on Alma through singularity. 
Additionally, many bioinformatics tools come available as docker images that can be run on Alma though singularity.
Instructions are here: [Docker and Singularity](workflows/containers.md)

---  

For any help or questions email [scientific computing](mailto:schelpdesk@icr.ac.uk).

---  