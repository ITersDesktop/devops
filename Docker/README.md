
# Docker: Recipes, Tips and Tricks

### R001: How to check the size of an image
**Context**: We need to know the size of an image after building. The image is identified via tags.
**Solutions**:
```
docker image ls --format "{{ .Size }}" tnguyenv/ssdemo:1.1-dev
```

### R002: How to build the smallest image
**Context**:
During the process of building docker images, we often install required packages so that it will add more layers into the final image. Therefore, the size of image is significantly bigger.

**Solutions**
I read [this writing](https://pythonspeed.com/articles/system-packages-docker/) that I am thinking useful and good work. There is [must-read post](https://learnk8s.io/blog/smaller-docker-images) precising how docker layers are built to help us avoid expanding the image size unnecessarily.

### R003: How to tag an existing image
**Context**
I had built an image tagged tnguyenv/ssdemo:1.1-dev, however, I want to push it to gitlab container registry hosted and managed internally in my organisation. How to proceed that need?

**Solutions**
Using `docker tag` command to assign a specific tag to an existing image. The syntax looks like
```
docker tag tnguyenv/ssdemo:1.1-dev dockerhub.ebi.ac.uk/biomodels/spring-session-demo:1.1-dev
```

### R004: How to install some essential packages for troubleshooting
**Context**
We want to ping a domain name to check whether the internet works or not. Especially, it would be worth to know as if the application is listening an exposed port or not. One of the common need is to edit files using vim when vim is not installed default due to optimisation.

**Solutions**
***Approach 1: Log in into the container and install networking package***
```
docker exec -it <container> bash
apt-get update
apt-get install net-tools vim
```
***Approach 2: Install networking package when building image***
This is a sample of the installing snippet (studied from [the answer](https://stackoverflow.com/a/30859601/865603)).
```
FROM python:3.7
LABEL maintainer="nguyenvungoctung@gmail.com"

RUN ["apt-get", "update"]
RUN ["apt-get", "install", "-y", "vim"]
```
To optimise the image in question, we should define a long command as below.
```
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    net-tools vim \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
```
### R005: How to update a file in the container if it was not installed any text editor
**Context**
If we cannot install vim in a container because of permission denied, how can we update a non-trival file.

**Solutions**

***Approach 1*** Edit the file locally then copy it to the container.
This approach is suggested from [this answer](https://stackoverflow.com/a/48397043/865603) or [that answer](https://stackoverflow.com/a/39399158/865603) in Stackoverflow. We can edit the file in the host, then copy it to and run it inside the container.
```
docker cp main.py my-container:/data/scripts/
docker exec -it my-container python /data/scripts/main.py
```
***Approach 2*** Edit the files over SSH from the Docker host to the container
This approach is recyled from [the answer](https://stackoverflow.com/a/43548695/865603) in Stackoverflow. To keep your Docker images small, don't install unnecessary editors. You can edit the files over SSH from the Docker host to the container.
```
vim scp://remoteuser@containerip//path/to/document
```

### R006: How to tell docker container connecting to the databasee server of host machine ###
**Context**: If you want to gather data in a unique place, thee database server should be installed on the host machine so that all applications running on docker containers are able to connect to. How to make it done?

**Solutions**

***Approach 1*** Stealing [the answer](https://stackoverflow.com/a/60116841/865603), I suggest adding the following line as databasee url of DataSource in your applications.

```
spring.datasource.url=jdbc:mysql://host.docker.internal:8889/apidb
```

