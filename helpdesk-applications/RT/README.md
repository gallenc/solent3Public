# RT Request Tracker (*Currently not working)

## Introduction

**0. Install CentOS** and, if necessary, update the runc command (```sudo yum install runc --allowerasing```)**

**1. Install Docker Engine**: 

| Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | ```sudo yum install -y yum-utils```  | Install Yum Utils (Required by Docker) |
| 2  | ```sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo```  | Add copy of Docker repo to Yum Config |
| 3  | ```sudo yum install docker-ce docker-ce-cli containerd.io``` | Use YUM to install Docker (Docker is subsequently downloaded automatically) |
| 4  | ```sudo systemctl start docker``` | Start the docker engine. *This must be done after every system boot-up!* |
| 5 | ```sudo sysctl -w vm.max_map_count=262144``` | Increase the maximum memory for virtual machines on linux (to at least 4 GiB) as ELK Stack requires minimum 4GiB. *This must be done after every system boot-up!* | 

\*Please Note: The commands at each step are all just single commands, but some may end up being across multiple lines here on Github if they're quite long!


**2. Run RT in Docker**

Use the command `sudo docker run --name rt -p 80:80 netsandbox/request-tracker:latest` to run RT in a Docker container (named "rt" on port 80), using the lastest Docker Image.

In order to restart RT - after a reboot, for example - you can use the command `sudo docker restart rt`.

RT can now be accessed in your browser at `localhost`

## Demo Installation
