# Prometheus

## Introduction

**0. Install CentOS** and, if necessary, update the runc command (```sudo yum install runc --allowerasing```)

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


**3. Download Template**

Download the `promethius template.zip` and extract the contents into the directory you wish to run Prometheus in, and navigate to this location in the linux terminal. Run the application in Docker using the command below, and wait for start up to complete.
 
 | Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | ```sudo docker-compose up```  | Run the project. Docker will automatically download and install any modules named in the Docker-Compose.yml file which aren't yet downloaded. In this case that'll probably be all of them. Don't worry though, it'll only download them the first time! |

You can now head to `http://localhost:3000` in your browser and login with the username and password `admin` and `foobar` respectively (these can be changed in the `docker-compose.yml` file) to see some host metrics.


**4. Collect and View Docker Metrics with Prometheus*

In order to track data other than host data, nodes need to be set up to send data to Prometheus. This can be done in different ways with different applications sending different metrics etc., however in this case we will collect the Docker metrics from the host machine.

To do this, locate the file `/etc/docker/daemon.json` and (if it exists) copy it to a directory you can write files to (since it's read-only here). Then,add the following: ``
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}``

Next delete the original file via `sudo rm /etc/docker/daemon.json` (you'll need to confirm by entering `y` when prompted) and then move your edited version using `sudo mv *path-to-edited-file* /etc/docker/daemon.json`

Then, open the Prometheus config at `prometheus-run-directory/prometheus/prometheus.yml` and paste the following into the `scrape_configs` section:

```
  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9323']
```

Lastly, stop Prometheus and restart the Docker Engine using the command `sudo systemctl stop docker` then `sudo systemctl start docker`. Start Prometheus again and you will be able to view Docker Metrics by going to Dashboards > Manage Dashboards and selecting the "Docker Prometheus Monitoring" dashboard.

## Demo Installation
