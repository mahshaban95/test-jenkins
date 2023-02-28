## Steps to install Jenkins on an Ubuntu machine

- Create an Ubuntu 22.04 LTS machine.

- Jenkins runs as a standalone application.

- The Jenkins WAR file bundles Winstone, a Jetty
    - Winstone:
    - Jetty:

- Install using Docker:

```bash
# Pull LTS Jenkins image
docker pull jenkins/jenkins:lts-jdk11

# Create a bridged network for Jenkins using Docker
docker network create jenkins

# Run Docker container DiND
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2

# What is dind?
# https://shisho.dev/blog/posts/docker-in-docker/


# Create a dockerfile for the jenkins image
# Build the image as below
docker build -t myjenkins-blueocean:2.375.3-1 .
# The image is just a Jenkins image with docker installed on it

```



## Steps to create a Jenkins machine on Linode

- Go to Linode > Marketplace > Search for Jenkins

- Choose the image, region, and plan for the server

## Access Jenkins

```bash

# SSH into the server 
ssh root@<ip or host name>

# Get the inital token for jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword

# Open jenkins in browser
# http://<ip or host name>:8080/
# Copy the initail token to log int the server

```

## Guided tour

Guided tour uses Blue Ocean 
- Blue Ocean as it stands, provides easy-to-use Pipeline visualization. 
- Blue Ocean will not receive additional functionality or enhancement updates. [(Reference)](https://www.jenkins.io/doc/book/blueocean/#blue-ocean-overview)
- We may want to consider alternative options such as the Pipeline: Stage View and Pipeline Graph View plugins.

Install suggested plugins, and create credentials.

Follow guide. [(Link)](https://www.jenkins.io/doc/pipeline/tour/hello-world/)

Jenkins Pipeline
- Jenkins Pipeline (or simply "Pipeline") is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins.

Create your first pipeline:
- Install the Docker Pipeline plugin (Manage Jenkins > Manage Plugins).
- Write your Jenkinsfile and push it to your repository.
- Go to Jenkinks > Create a new item > Name the pipeline.
- Choose the pipeline type:
    - Freestyle project: Totally ad-hoc. Combines any SCM with any build system. 
    - Pipeline: Orchestrate long-running and complex activities.
    - Multi-configuration project: Suitable for working with different environments with different configurations.
    - Folder: Provides more organization for projects to be in different namespaces instead of just relying on filters.
    - Multibranch Pipeline: Suitable for dealing with and detecting multiple in one single SCM repo
    - Organization Folder: Organizes multibranch projects in separate folders.
- We will choose "Multibranch Pipeline" and press "OK"
- Go to "Branch Sources" to add your repo info.
- Jenkins will automatically detect any new Branches or Pull Requests that are created in your repository and start running Pipelines for them.








