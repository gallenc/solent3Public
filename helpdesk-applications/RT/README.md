# RT Request Tracker

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


**2. Install Docker Compose**: 

| Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | ```sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```  | Download the Docker Compose (1.29.2) and install it locally. |
| 2  | ```sudo chmod +x /usr/local/bin/docker-compose```  | Add executable permissions to the downloaded binary... so, you know, you can run it. |
| 3  | ```sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-composer``` | Create a symbolic link at the global install directory to the local user install. This is needed because as Docker was installed to your user, you can't natively "sudo" Docker Compose (as sudo can only access global commands). By creating a symbolic link at the global location, we're telling the Sudo command where it should actually find Docker Compose. |

\*Please Note: The commands at each step are all just single commands, but some may end up being across multiple lines here on Github if they're quite long!


**3. Download and Extract Docker-Compose Repo**

Head to *[https://github.com/FireFart/rt-docker](https://github.com/FireFart/rt-docker)* and download the whole repo. Extract it to the directory you wish to run RT in, and navigate to this location in the linux terminal. Then run the the command `openssl req -x509 -newkey rsa:4096 -keyout ./nginx/certs/priv.pem -out ./nginx/certs/pub.pem -days 3650 -nodes` and follow the instructions. This creates the relevent public and private certs for nginx.

Next, you'll need to create a number of config files. For now, we'll just use the example ones which came with the repo; to do this we have to rename and move some files around. To make the changes necessary, use the following commands to move and rename files:
 
 | #  | Command* |
| ------------- | ------------- | 
| 1  | ```mv RT_SiteConfig.pm.example RT_SiteConfig.pm```  | 
| 2  | ```mv getmailrc.example getmail/getmailrc```  | 
| 3  | ```mv msmtp.conf.example msmtp/msmtp.conf```  | 
| 4  | ```mv contrab.example contrab```  | 

If you need to change the configuration of RT, you can do by altering those files.

Now, run RT using the command `sudo ./prod.sh`






## Demo Installation

## Sources

**1. [https://github.com/FireFart/rt-docker](https://github.com/FireFart/rt-docker)**
