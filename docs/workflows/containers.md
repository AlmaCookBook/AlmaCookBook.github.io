# Singularity and Docker  

There are times when you only have access to docker, not singularity, for example when developing on your own workstation. 

There are times when you only have access to singularity but not docker, for example when using Alma.

**This article covers using docker images in both docker and singularity.**

### Devolping a custom docker image

If you develop a custom docker image and upload it to dockerhub, it will be available to pull from docker in the standard way. An example of such an image managed by the ICR Software Group is: [pisca-run](https://hub.docker.com/r/icrsc/pisca-run)

Pull with docker or singularity
```
docker pull icrsc/pisca-run
singularity pull docker://icrsc/pisca-run
```

The difference when pulling with singularity is that you provide the docker url.

Generally, you just add **docker://** in front of the image name.

### Running with docker or singularity

Note - you do not need to pull first, if you don't it will pull as part of the first run call, which means that will take longer.

For this particular image:  
The docker command is  
```docker run -v /your/xml/dir:/mnt icrsc/pisca-run your.xml```  

The singularity command is:  
```singularity run -B /your/xml/dir:/mnt docker://icrsc/pisca-run your.xml```  

There are slight differences in the parameters between singularity and docker, in this case passing a mounted directory as a different parameter letter. Overall though, it is just the addition of **docker://** in front of the image name.

