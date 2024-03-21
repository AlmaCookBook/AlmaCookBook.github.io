# Packaging Code for re-usability

## Introduction
Packaging is the process of bundling your code into a distributable format that can be installed and used by others. Packaging your code makes it easier for others to use and contribute to your project, and helps ensure that your code is reproducible and maintainable. 

The appropriate package will depend on how you want the code to be used, but equally the available packages and best practices could influence how you design your code.

## Libraries
In both R and python libraries can be created to package code. In python these packages can be installed with `pip install` and in R with `install.packages`.

## Containers
Containers are a way of packaging code and its dependencies into a single unit that can be run in any environment. Containers are lightweight, portable, and can be run on any platform that supports the container runtime. Some popular container runtimes include Docker and Singularity.

## Web Applications
Web applications can be packaged as Docker containers, which can be deployed to a cloud platform such as AWS, Google Cloud, or Azure. This allows you to easily scale your application, and ensures that it runs consistently across different environments.

## Packaging at the ICR
At the ICR we use a combination of Python packages and Docker containers to package our code. Python packages are used for libraries and command-line tools, while Docker containers are used for web applications and services. We also use Github Actions to automate the packaging and deployment process, which ensures that our code is packaged consistently and reproducibly.

We have developed a model of 3-tier packaging for our code that enables users to go from minimum-requirements to expert HPC. For this we use:
- A webapp hosted at the ICR - light weight for explaration only
- A docker container that can be run on any machine for heavier use of the application
- A docker command line tool of the same code for more intensive local use
- That same command line util can be run though singularity on Alma

Please contact schelpdesk@icr.ac.uk if we can help with any aspect of packaging your software.

An example of a project with this multi-tier packaging is pisca-box.

- Public web application [Pisca-box website](https://software.icr.ac.uk/app/pisca-box)
- Docker container for web app [Pisca-box docker container](https://hub.docker.com/repository/docker/icrsc/pisca-box/general)
- Docker container for command line tool [Pisca-run docker container](https://hub.docker.com/repository/docker/icrsc/pisca-run/general)

