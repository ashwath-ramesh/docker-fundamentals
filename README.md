# Fundamentals of Docker

- [Quick start](quick_start.md)
- [Creating and using containers](creating_and_using_containers.md)
- [Container images](container_images.md)
- Persistent data
- Docker compose
- Swarm
- Container registries
- Docker in production
- Docker security
- Automated CI workflows
- Github actions

## Install docker

`sudo apt install docker.io`

`systemctl status docker`

`sudo systemctl enable docker`

`sudo systemctl start docker` - docker starts automatically when we start server

`sudo docker run hello-world` - test that the install docker works. This checks local docker for this container, and if not found, pulls the image and runs it.

`sudo usermod -aG docker tom` - Make sure we don’t need sudo to run. Logout and login. Reboot if needed.

## Running containers

`docker images` - show all docker images locally available on machine

`docker search ubuntu` - search for ‘ubuntu’ image from docker daemon

`docker pull image` - Download image ubuntu

`docker run ubuntu` - name of image from which we want to create a container. Seems like nothing happened. That’s because there is nothing to run, and hence docker container exited.

`docker run nginx` - has an inbuilt command that immediately starts an application inside the container, here the nginx web server.

`ctrl + c` - exit

`docker ps` - list of all containers currently running on system

`docker ps -a` - list of all containers currently running on system & one that previously ran

`docker run -it ubuntu /bin/bash` - Force a container to keep running. -it is interactive mode.

Now create a folder called test in /home/

`ctrl +d` - disconnect from shell

Changes need to be saved in the image.

## Making containers persist

`docker run -it -d ubuntu` - run the container in the background. -d means run in daemon.

`docker attach <container id>`: then gets you attached to that container.

When in interactive shell mode inside a docker image, then instead of exiting using `ctrl +d` , use `Ctrl + p, q` to persist the container.
CAUTION! Using `ctrl + d` will destroy the container.

## Access containerised apps

`docker run -it -d -p 8080:80 nginx`

- -p: port
- 80: nginx runs on port 80 by default and is listening for connections. But ports inside a container are not available by default outside the container. And we need to give a host mapping from the docker host.
- 8080: we are running docker on this port

`ip a`: get IP address of host system

Paste `111.111.11.11:8080` on browser

`docker stop <container id>`: stops and terminates docker container

`docker run -it -d - -restart unless-stopped -p 8080:80 nginx`

Now this docker runs even if ctrl +c is used. It only stops only when stop command is used.

## Creating images

`docker run -it ubuntu`

`apt install apache2`

`/etc/init.d/apache2 status` : check if apache is running

`/etc/init.d/apache2 start` : start apache2

`Ctrl p,q`

`docker commit <container id> <repo_name/image_name:version>` :convert running container to an image

`docker commit 90173b88eb2c repo/first-test:1.0`

`docker images` : show images

`docker run -d -it -p 8080:80 lux/apache-test:1.0` : Create a container from an image. We map 8080:80 to this apache web server that is running inside a container.

But If you go to `111.111.11.11:8080` on browser, nothing works. That’s because we did not set an entry point.

Entry point - what command should we run when we start a docker container.

So recreate the image:

`docker commit --change='ENTRYPOINT ["apachectl", "-DFOREGROUND"]' <ID> repo/first-test:1.1`

## Creating a container image from a Dockerfile

The following commands will enable to to create a container image similar to the previous one, but we’ll use a Dockerfile instead. A Dockerfile can contain all the commands we executed manually one-by-one, in one text file.

### Dockerfile

Save the following code into a text file, named “Dockerfile”:

```
FROM ubuntu
MAINTAINER tom <tom@somewhere.net>

# Skip prompts
ARG DEBIAN_FRONTEND=noninteractive

# Update packages
RUN apt update; apt dist-upgrade -y

# Install packages
RUN apt install -y apache2 vim-nox

# Set entrypoint
ENTRYPOINT apache2ctl -D FOREGROUND
```

### Build a container image from the Dockerfile

The following command will build a new container image, with our Dockerfile. Take note of the incremented version number, and also the period at the end. It must be run from the same location that contains the Dockerfile.

`docker build -t repo/first-test:1.2 .`

`docker build -t repo/first-test:1.2 .`

Remove docker image: `docker rmi <image_id>`

Remove docker container: `docker rm <container_id>`
