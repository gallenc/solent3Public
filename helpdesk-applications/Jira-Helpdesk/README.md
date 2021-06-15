# Jira Helpdesk

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


**3. Download and Extract Docker-Compose File**

Download `docker-compose.yml` and place it into the directory you wish to run Jira in, and navigate to this location in the linux terminal. Then run the application in Docker using the command below; this will run a standard Jira setup.
 
 | Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | ```sudo docker-compose up```  | Run the project. Docker will automatically download and install any modules named in the Docker-Compose.yml file which aren't yet downloaded. In this case that'll probably be all of them. Don't worry though, it'll only download them the first time! |

\*Please Note: The commands at each step are all just single commands, but some may end up being across multiple lines here on Github if they're quite long!

**4. Completing the Browser-Based Jira Setup**

On first run, you will need to register your Jira Setup. In your browser, head to http://127.0.0.1 (*or the IP of your JIRA setup if different*) and follow the on-screen process, including generating a trial license for Jira when needed. **WARNING: Do not proceed to creating the admininstrator account until you have saved any necesarry work in the background, and terminated any processes which may corrupt: When I set this up with 4GB of memory dedicated to it, creating the administrator account hung the machine, and I had to force reset it after ~20mins of waiting. If this happens to you also, simply restart the Docker Engine (Steps 4 and 5 of Part 1) and run Jira again, and you should find that (perhaps if you waited a few minutes before resetting) the setup process completed before the system hung.**[citation needed]

**Appendix A: Running Jira and ELK Stack in the same Docker-Compose environment**

By default, Jira and ELK Stack use different ports, so combining the two into a single Docker-Compose environment is as simple as copying the contents of the ELK Stack `docker-compose.yml` into that of Jira[citation needed]. You will, however, need to merge the volumes and networks section at the bottom. If you're having problems doing this, refer to the conents of the drop down below.

<details><summary>Merging of Network and Volumes nodes for combined docker compose file</summary>
  
  ```
  volumes:
    jiradata:
      external: false
    postgresqldata:
      external: false
    data01:
      driver: local
    data02:
      driver: local
    data03:
      driver: local

  networks:
    jiranet:
      driver: bridge
    elastic:
      driver: bridge
  ```
  </details>



## Demo Installation


## Sources

**1.** https://docs.docker.com/engine/install/centos/ 

**2.** https://docs.docker.com/compose/install/ 

**3.** https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html 

**4.** https://github.com/teamatldocker/jira



**Note: *[citation needed]* indicated statements that are either theoretical, or issues that may have been one-off occurances and need verifying as correct/repeatable.**
