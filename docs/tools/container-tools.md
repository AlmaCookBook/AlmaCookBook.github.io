# Using Scientific Tools with Singularity
Some container tools are installed as .sif files on Alma and some are on dockerhub, where they can be used either locally with docker, or on Alma via singularity. These examples have been built at the ICR in order to simplify the usage of the complex environments that are required for them.

Note - singularity is only installed on the compute/interactive modes. It is not available on the login nodes.  

---  

### bcl-convert from illumina
The software is from [illumina](https://emea.support.illumina.com/sequencing/sequencing_software/bcl-convert.html)
The image is on Alma, located at: /opt/software/containers/singularity/build/bcl.sif  

To use it, you need to bind a path on scratch to the container. It will use that path to write out log files. The general command is (asking for the help):

```bash
singularity exec --bind /path/to/your/logs:/var/log/bcl-convert /opt/software/containers/singularity/build/bcl.sif bcl-convert --help
```

If I want to check it works for, call help and bind it to your home path:
```bash
singularity exec --bind ~:/var/log/bcl-convert /opt/software/containers/singularity/build/bcl.sif bcl-convert --help
```
Note the --bind argument is obligatory, the container has hard coded into it the requirement to pass in a path to /var/log/bcl-convert. If you don't do this you will get an error.

---  

### pisca from the ICR
The image is on dockerhub, located at: [docker://icrsc/pisca-run](https://hub.docker.com/repository/docker/icrsc/pisca-run/general).  It was created for Heather Grant by the RSE team in Scientific Computing.  

Pull it first into the directory you want to run it from, and then run it with a call that binds the directory in which you have the xml files to the container.  The general command is:
```bash
singularity run -B /your/xml/dir:/mnt docker://icrsc/pisca-run your.xml
```
---  

