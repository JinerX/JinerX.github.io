---
title: 'Docker Operations Cheatsheet'
date: 2026-03-26T12:21:52+01:00
draft: false
tags: ["docker", "devops"]
math: true
---



## Theory
- namespaces       - abstraction which makes it possible for the system to isolate processes from one another. Processes internally store a pointer to a namespaces when calling specified commands "dealing with" a specific kernel resource it first checks for appropriate namespace and returns appropriate data. Some important namespaces:
    - PID namespaces - process isolation - container can only see processes made inside the container
    - network namespace - netowork isolation - each container gets it's own ports,ip addresses, etc. Connected via `docker0` bridge to the host network.
    - mount namespace - filesystem isolation - each container gets it's own filesystem hierarchy, mount points etc.
- cgroups - used to limit the system resources available to the container, CPU, memory etc.
- union mount filesystem -  filesystem technique used in docker containers. We define the filesystem in multiple layers (defined typically in `Dockerfile`) during build we only look at the "top". If conflicting upper layers overwrite the lower layers. All layers beside the top one are read only, the top one is read/write.
- docker daemon - runs in the background and manages the containers, volumes etc. We communicate with it via CLI. If docker is installed on linux we communicate with it via exposed socket at `/var/run/docker.sock`. If installed on a different system a virtual machine with linux is setup and we communicate with the docker via socket at the same location but within the VM.  


##  Basic structures
- images     - contain information about how to build the container. 
- containers - working instances where we actually operate and that do the "actual" work.
- volumes    - data storage objects can be mounted onto containers 
- networks   - define how containers communicate with each other 

## Basic commands
with all of those `--help` flag lists all available commands with a short description
- docker images:
    - `docker image ls` - list all images on the system
    - `docker pull <image_name>` - download the image (by default from dockerhub)
    - `docker image rm <image_name>` - remove an image
    - `docker scan <image_name>` - scan for vulnerabilities
    - `docker history <image>` - show image layer history
- docker containers:
    - `docker start <container>` - start a stopped container
    - `docker stop <container>` - stop a running container
        - `-t N` - wait N seconds before killing
    - `docker restart <container>` - restart a container
    - `docker logs <container_name>` - show container logs
        - `--tail N` - last N lines 
    - `docker ps` - show running containers
        - `-q` - only container ids
        - `-a` - all containers (not just running)
    - `docker top <container>` - shows processes running inside a container
    - `docker exec <container> <cmd>` - run command inside running container
        - `-it` - interactive shell (e.g. `/bin/bash`)
    - `docker attach <container>` - attach to container STDIN/STDOUT
    - `docker rm <container>` - remove container
        - `-f` - force remove (even if running)
        - `-v` - remove associated volumes
- docker volume
    - `docker volume ls` - list volumes
    - `docker volume rm <volume>` - remove volume
    - `docker volume create <name>` - create a volume

- docker network
    - `docker network ls` - list all networks
    - `docker network create <name>` - create a new network
        - `-d <driver>` - specify driver (e.g. bridge, host, overlay)
        - `--subnet <subnet>` - define subnet 
    - `docker network rm <network>` - remove a network

- misc:
    - `docker info` - show system wide docker info
    - `docker inspect <image/volume/container/network>` - show detailed information about the object
    - `docker stats` - system wide resource usage
    - `docker <image/container/volume/network/system> prune` - remove dangling objects of a given type.
        - `-a` - remove all unused, not just dangling
    - `docker system df` - show disk usage 
 
Other commonly used are:
- `docker run`
- `docker build` 
- `docker compose`
They are described in later sections.


## storing data
- volume commands:
    - `docker run ... --mount source=<volume_name>,destination=<destination_path>` - mount a specified volume at a destination
    - `docker run ... -v <volume_name>:<destination_path>` - shorter version
- bind mounts - if we want to use host's folder for storage within the container
    - `docker run  ... -v ${PWD}:<destination_path>`
- environment variables:
    - `docker run -e <VARIABLE>=<value> <image>` - pass single EV
    - `docker run --env-file .env <image>` - using `.env` file


## docker build
`docker build [OPTIONS] <path>` is a command used to create an image from a *Dockerfile*. `path` is the location of the Dockerfile.

### build context
The path in the command is also a path to the build context. Those are the files which docker can use during the build process. It can run commands like `COPY` or `ADD` only on the files in the build context.

>[!warning]
Docker sends all the files in the build context to the docker daemon. So if the size of files is large it may take a long time.

### flags
- `-t` - changes the name of the resulting image
- `-f` - specify a dockerfile in a different location/name
- `--build-arg` - pass build time arguments
- `--no-cache` - disable cache and force all the build steps to run fresh
### dockerignore

A special file call `.dockerignore` files specified in it will not be included in the build context even if they are in the path.

### Dockerfile
- `FROM <base_image>` - base image which will serve as base layer of our image. 
    - `as <name>` can be added to refer to it later  for multi stage build
- `RUN <instruction>` - run some command in the docker shell
- `COPY [OPTIONS] <src> ... <dest>` copies from `<src>` and adds them to the filesystem ad `<dest>`
- `CMD <command> <params>`  - sets the command to be executed when running container **at runtime** from an image, there can only be one `CMD` per Dockerfile
- `WORKDIR <dir>` - change the current working directory in future instructions
- `USER <user>`  - change the user
- `ENV <key>=<value>` - sets environment variables
- `EXPOSE <port>` - describes which port the application is listening on


## Running containers
### docker run
most used flags:
- `-d` - detach, run in background
- `--entrypoint` - overwrite entry point
- `-e` - set environment variable
- `it` - shell session in the container
- `--name` - name a container
- `--network` - set a netowrk
- `--rm` - if exited delete the container
- `--restart` - set the policy for when to the container should automatically restart

### docker compose
Internally the same as docker run, but instead of specifying all the flags it reads it from `docker-compose.yml`

- `docker comopose up` - to start a container
    - `-d` - for detached
- `docker compose down` - stop and remove containers

#### docker-compose.yml
https://www.composerize.com/ - pass `docker run` and get corresponding `yaml` file


- sevices - here you define containers, each service - one container
- `app` - name of the service
- `image:` - which image to use if it's not on the machine it will be pulled
- `ports:` - maps ports between machine and container format: `HOST:CONTAINER` - localhost:8080 - inside of container is 80 so at localhost:8080 we'll see the nginx running
- `build:` - instead of pulling image build from dockerfile
- `volumes: - <volume_name>:<destination>`
- `volumes: <path>:<destination>` - for bind mounts

Example:

``` yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - ./app:/app
    environment:
      - DATABASE_URL=postgresql://admin:secret@db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:15
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret

volumes:
  db_data:
```




































