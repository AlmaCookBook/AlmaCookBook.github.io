# First Steps to using Alma

The objective of this section is to briefly present the different software tools which are made available on Alma HPC.
This includes: code versioning tools, Alma GUI version or AlmaOnDemand, package manager tools, among others.

It is recommended to set up carefully these tools prior to running scripts on Alma. 
This will ensure that the majority of the further help tutorials are accurate for your environment.

If you don't think any of this is going to be necessary, feel free to skip it.  

1. **Github account**  
If you don't have a github account, you can create one [here](https://docs.github.com/en/get-started/onboarding/getting-started-with-your-github-account). 
Use your ICR email address for the GitHub account, and once you have one raise a ticket with the 
[scientific computing helpdesk](mailto:schelpdesk@icr.ac.uk) and ask to be added to the ICR GitHub organisation.


2. **Git with ssh keys**  
Many applications are integrated with git and having it set up correctly from the outset will save problems down the line. 
You want to have it set up with ssh access from Alma to GitHub. 
    - Check first if you [have ssh keys set up](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys).  
    - Otherwise [generate a new one](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).
    - Then [add the ssh key to github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

3. **The Alma fileshare**  
Samba servers exist for mounting easily a remote system on your Machine, for both SCRATCH and RDS.
This allows you to move files between your local machine and Alma and to edit files directly on Alma. If you prefer (or need to access home), there are various browser applications such as WinSCP for the file system.

> Note for MAC users: Press Command+K and enter the Samba server address or simply open the address in your browser to mount the remote system.  

- **IOS**  
To have quick access to the scratch and RDS, save scratch and RDS server links in you favourites. You can do so by going 'Finder'>'Go'>'Connect to server...' and typing in the one of the following server addresses:
  - For scratch: smb://alma-fs
  - For RDS: smb://rds.icr.ac.uk/DATA

  Save the server by pressing the '+' icon. Whenever you want to connect, repeat 'Finder'>'Go'>'Connect to server...' and choose the server you wish to connect to.

- **Windows**  
Using file explorer, right-click on 'This PC' and choose "Add a network drive". Enter these paths:  
  - For scratch:  `\\alma-fs\SCRATCH`  
  - For RDS: `\\rds.icr.ac.uk\DATA`  

4. **Conda and Mamba**  
You should initialise mamba and conda in your shell profile. This will make sure that the conda and mamba commands are available in your shell.  
Instructions are here: [Conda and Mamba Initialise](conda/mamba-first.md)

5. **Python and R OnDemand**  
Make sure you can use the ondemand applications you will require here: [Alma Open OnDemand](https://alma-ondemand.icr.ac.uk).  
RStudio uses the Alma installation of R and does not allow for environments (unlike R in scripts which can be used in a mamba environment) 
but [jupyter notebooks can be used with a conda environment](conda/python-ondemand.md).  

6. **Nextflow**  
Alma has a shared installation of Nextflow, but you can also access nextflow through python virtual environments or conda environments.
Instructions are here: [Nextflow](workflows/nextflow-envs.md)  
Nextflow is used to build analysis pipelines among others. A community effort was put to collect existing set of analysis pipelines and to save in 'nf-core' pipelines.
nf-core pipelines are also available on Alma, the complexity is in the way the pipelines are run on the slurm executor.
Instructions are here for [getting started](workflows/nf-core-1.md) and [running an nf-core pipeline](workflows/nf-core-2.md).

7. **Docker and singularity**  
An alternative to conda environments is creating a docker image and running it on Alma through singularity. 
Additionally, many bioinformatics tools come available as docker images that can be run on Alma through singularity.
Instructions are here: [Docker and Singularity](workflows/containers.md)

---  

For any help or questions email [scientific computing](mailto:schelpdesk@icr.ac.uk).

---  

8. **Connection to Alma and using Slurm**
Alma clusters are hosted on : alma.icr.ac.uk.

You can access your /home/ repository via ssh, as follows:
```bash
ssh alma.icr.ac.uk
```
Alma is using Slurm queue system for running jobs through scheduling. 
Users submit jobs, which are scheduled and allocated resources (CPU time, memory, etc.) by the resource manager.

Multiple partitions exist on Alma clusters: interactive, compute, GPU, data-transfer, short, among others.
Compute queue is the default one. Depending on the task to accomplish one partition could be more adapted than the others. For instance, use data-transfer partition to move files, use GPU partition to run GPU adapted software/code and use interactive or short partition for ephemeral tasks.

It is important to note that any computation should not be made directly on the cluster head, but rather on a node.
Below is an example of how to run an interactive session on a remote node, for 2h with 10GB max memory and 2 cores.
```bash
srun --pty --mem=10GB -c2 -t 02:00:00 -p interactive bash
```
A good practice is to set accurately the allocated resources (CPU time, memory and core numbers) and not to over-estimate them to get a chance to run fast your job. The node you connect to will depend on the resources you request, usually node01 if you require more compute power and node24 is less. 


For more info,  an extensive internal documentation is present on [Nexus](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/Pages/New-Users-Guide.aspx)