# Learn Docker in x minutes

[TOC]

## What's Docker

Docker is a platform for developers and sysadmins to develop, ship, and run applications. It's created in the early of 2013 and based on Linux container LXC.

Docker makes things easy and convienent:

* Build: Develop an app using Docker containers with any language and any toolchain.
* Ship: Ship the “Dockerized” app and dependencies anywhere - to QA, teammates, or the cloud - without breaking anything.
* Run: Scale to 1000s of nodes, move between data centers and clouds, update with zero downtime and more.

Docker consists of:

* The Docker Engine: Lightweight and powerful open source container virtualization technologyl
* [Docker Hub](https://hub.docker.com/): The central hub for Docker. It hosts public Docker images and provides services to help build and manage Docker environment.

## Installation

Please refer to [Docker Installation][install]. Here just gives some guide for Arch Linux.

Install docker package in community:

```sh
$ sudo pacman -S docker
```

**Start docker**

```sh
$ sudo systemctl start docker
```

To start on system boot:

```sh
$ sudo systemctl enable docker
```

If you want to make customizations on daemon options, please refer to [customize your systemd Docker daemon options](https://docs.docker.com/articles/systemd/).

## Docker Hub

**Creating an account**

```sh
$ sudo docker login
```

## Dockerizing Applications

Docker provides ability to run applictions inside containers. Run an application with command `docker run`.

### Hello World

Say 'Hello world' to docker:

```sh
$ sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
Hello world
```

This command may take some time to download the docker image at first time. `ubuntu:14.04` is the image which is the container we ran. Docker looks for the image on your Docker host first. If the image is not found, it will be downloaded from Docker Hub.

Finally, docker executes command `/bin/echo 'Hello world'` inside the container we launched.

### An Interactive Container

This time we try a new commmand:

```sh
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
root@609bc7a62c87:/#
```

Here adds two options to `docker run`:

* -t: Assign a pseudo-tty or terminal inside the container.
* -i: Make an interactive connection by grabbing the standard in (STDIN) of the container.

Let's execute some commands:

```sh
root@609bc7a62c87:/# cat /etc/issue
Ubuntu 14.04.1 LTS \n \l

root@609bc7a62c87:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@609bc7a62c87:/# exit
```

### A Daemonized Hello world

Now create a container running as a daemon:

```sh
$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
c01af8ac6ed3cb0f314f82806feaf23ac8b8fc232a2a0c8286878768b4f7af5e
```

The `-d` tells docker to run the container in the backgroud. And docker returns a long string which is a *container ID*.

We can query Docker daemon with `docker ps`:

```sh
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
c01af8ac6ed3        ubuntu:14.04        "/bin/sh -c 'while t   9 minutes ago       Up 8 minutes                            distracted_nobel
```

See output using `docker logs` command with the name assigned automatically by docker:

```sh
$ sudo docker logs distracted_nobel
hello world
hello world
hello world
...
```

## Reference

1. [Docker][docker]
2. [Docker Docs][docs]
3. [Docker Installation][install]

[docker]: https://www.docker.com/
[docs]: https://docs.docker.com/
[install]: https://docs.docker.com/installation/
