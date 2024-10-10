# Singularity and Docker  

There are times when you only have access to either docker or singularity.
For example, when using alma you only have access to singularity but not to docker.
Also when developing on your own workstation you can have access to docker only for instance.

**This article covers using docker images in both docker and singularity.**

### Developing a custom docker image

If you develop a custom docker image and upload it to dockerhub, it will be available to pull from docker in the standard way. An example of such an image managed by the ICR Software Group is: [pisca-run](https://hub.docker.com/r/icrsc/pisca-run)

Pull with docker or singularity
```
docker pull icrsc/pisca-run
singularity pull docker://icrsc/pisca-run
```

The difference when pulling with singularity is that you provide the docker url.

docker command requires a path while singularity command requires a url to the docker image.
Generally, the docker url consists of: **docker://** preceding the image name.

### Running with docker or singularity

The docker and singularity run command runs a command in a new container, pulling the image if needed and starting/launching the container.
Note that you can always choose to pull the image first, thus optimising the run command execution time.

For this particular image:  
The docker command is:  
```docker run -v /your/xml/dir:/mnt icrsc/pisca-run your.xml```  

The singularity command is:  
```singularity run -B /your/xml/dir:/mnt docker://icrsc/pisca-run your.xml```  

There are slight differences in the parameters between singularity and docker. 
For instance, passing a mounted directory is done using -v for docker and using -B for singularity. 
Overall though, it is just the addition of **docker://** in front of the image name.

