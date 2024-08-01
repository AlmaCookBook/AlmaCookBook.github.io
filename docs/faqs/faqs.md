# Frequently Asked Questions Using Alma

## 1. How do I get access to Alma?
Follow the [Getting Started](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/Pages/Getting-Started.aspx) tutorial on nexus.  
Also follow the [First Steps](../first_steps.md) tutorial on this site.

## 2. Who do I ask for help?
Send an email to [Scientific Computing HelpDesk ](mailto:schelpdesk@icr.ac.uk)

## 3. Disk Space Problems

The quota is 10GB. If you are running out of space, you can check your disk usage with the following commands:
```bash
du -sh .conda/
du -sh /home/ralcraft/
ls -al
```

## 4. ERROR: Could not install packages due to an OSError: [Errno 122] Disk quota exceeded: '/path/to/python3.10'  
Does conda need to be cleaned out? Check for conda cache files:
```bash
du -sh .conda/
conda clean --all
```

## 5. My singularity image did not pull!!! 
Occasionally large singularity images can cause network issues. Try again, or try with wget.  
If this failed:
```bash
singularity pull  --name depot.galaxyproject.org-singularity-mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb-7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0.img.pulling.1715165870091 https://depot.galaxyproject.org/singularity/mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb:7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0 > /dev/null
```
Then the image you were trying to pull is the second parameter in the command, so https:// etc.  
The name that you want this image to have is the first parameter in the command up to where it says pulling, so:  
- image = https://depot.galaxyproject.org/singularity/mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb:7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0  
- name = depot.galaxyproject.org-singularity-mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb-7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0.img

Replicate the command with wget like:  
```bash
wget https://depot.galaxyproject.org/singularity/mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb:7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0 -O depot.galaxyproject.org-singularity-mulled-v2-d9e7bad0f7fbc8f4458d5c3ab7ffaaf0235b59fb-7cc3d06cbf42e28c5e2ebfc7c858654c7340a9d5-0.img
```
It should go into your singularity or APPTAINER directory, which you should have set up in your home directory. To check it do:
```bash
echo $APPTAINER_CACHEDIR
echo $SINGULARITY_CACHEDIR
echo $NXF_SINGULARITY_CACHEDIR
```

Where APPTAINER_CACHEDIR is the new place singularity goes, and SINGULARITY_CACHEDIR is the old place, and NXF_SINGULARITY_CACHEDIR is the place nextflow looks for singularity images.

To change them do (for which ever one you want to change):
```bash
export APPTAINER_CACHEDIR=/data/scratch/your/path/username/.singularity/cache
```

## 6. My conda/mamba environment is broken
If you have a broken conda/mamba environment, you can try to fix it by removing the environment and creating it again. You can do this with the commands (conda or mamba):
```bash
conda env remove --name myenv
conda create --name myenv
```

However sometimes the environments are so badly broken you cannot use an conda commands at all and get unhandled or unknown excpetions. 
In these rare circumstances you can totally refresh your installation by:  
1. Renaming the .conda directory in your home directory  
2. Renaming the .condarc file in your home directory  
3. Remving the conda init section in your .bashrc file which means deleting this entire section  
```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
Delete all this stuff too, as well as the >>> conda initialize >>> line
and the <<< conda initialize <<< line
# <<< conda initialize <<<
```
4. Go though the [init process](../first_steps.md) to set up conda and your environment again.  

