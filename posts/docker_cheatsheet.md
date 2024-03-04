---
aliases:
- /markdown/2021/09/28/docker_cheatsheet
categories:
- Docker
date: '2021-09-28'
author: Zeel B Patel
description: Most used command while working with Docker
image: homepage-docker-logo.png
layout: post
title: Docker Cheatsheet
toc: true

---

## Images

One can download (pull) a docker image from [Docker Hub](https://hub.docker.com/) or respective websites:
```bash
# docker pull <image-name>:<tag>
docker pull tensorflow/tensorflow:2.4.0-gpu-jupyter
```

Check the downloaded images with:
```bash
docker images
```

Sometimes we may need to do something on an image without saving or alloting extra space. To create and run a temporary container to be deleted on stopping:
```bash
docker run --rm -it tensorflow/tensorflow:2.4.0-gpu-jupyter 
```

## Containers

Create a new container from an image with following flags
  1. ```-v```: for telling docker to use a shared directory between host and container
  2. ```-p \<host-port\>\<container-port\>```: is to use port forwarding, an application running on \<container-port\> can be accessed only with \<host-port\> in host.
  3. ```--name``` generates a name for the container for easy reference in other commands
  4. ```--gpus all``` tells docker to use all GPUs available on host
  5. ```--memory-swap``` Restricts RAM+Swap usage
  6. ```-it``` makes sure container awaits after starting instead of instantly shutting down if no startup scripts are configured.
  
To create new container with default params:
```bash
docker create tensorflow/tensorflow:2.4.0-gpu-jupyter 
```

To create new container with manual params:
```bash
docker create -it \
-v \path_in_host:\path_in_container \
-p9000:8888 \
--name aaai \
--cpus 2 \
--gpus all \ # To use specific gpus: --gpu '"device=0,2"'
--memory 90g \ # Uses 90g memory
--memory-swap 100g \ # --memory-swap is a modifier flag that only has meaning if --memory is also set. In this case 10g of swap will be used.
tensorflow/tensorflow:2.4.0-gpu-jupyter
```

Update some of the above configurations after container creation:
```bash
# change RAM limit of a container named "aaai"
docker update --memory-swap 50g aaai
```

> Note: In general, changes made to container persist when container is stopped.

Check containers:
```bash
docker ps # shows running containers
docker ps -a # shows all containers
```

Start a container (default script will be executed with this if any):
```bash
# docker start <container-name>
docker start aaai
```

Stop a container:
```bash
# docker stop <container-name> 
docker stop aaai
```

Delete a container:
```bash
# docker rm <container-name>
docker rm aaai
```

Go to a running container's shell:
```bash
#docker exec -it <container-name> bash
docker exec -it aaai bash # -it stands for interactive
```

Execute any command on a running container without opening a shell in container:
```bash
# docker exec -it <container-name> <command>
docker exec -it aaai jupyter notebook list
```

Check container logs (including shell commands output):
```bash
# docker logs <container-name>
docker logs aaai
```

## System

Check all images, all containers and space occupied by them:
```bash
docker system df -v
```

## Set up rootless docker

Refer to this guide: https://docs.docker.com/engine/security/rootless/

Main steps:

* Run `dockerd-rootless-setuptool.sh install`.
* Setup PATH and DOCKER_HOME as suggested by command output.
* `systemctl --user restart docker`.
* Try `docker images` to check if things worked.
* Try `docker run --rm hello-world` to check if things really worked.