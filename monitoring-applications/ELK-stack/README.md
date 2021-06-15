# ELK Stack (Elastic Logstash Kibana)

## Introduction


## Installation Guide

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

<details><summary>Installing ELK Item by Item yourself</summary>

**3. Install and run ELK Stack**
 
 | Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | n/a | Copy and paste the content of the dropdown named "Example Docker Compose File" below into an empty file, and save it with the name ```docker-compose.yml``` into the directory you want to run ELK Stack in. Navigate to the directory you want to install ELK Stack into. |
| 2  | ```sudo docker-compose up```  | Run the project. Docker will automatically download and install any modules named in the Docker-Compose.yml file which aren't yet downloaded. In this case that'll probably be all of them. Don't worry though, it'll only download them the first time! |

\*Please Note: The commands at each step are all just single commands, but some may end up being across multiple lines here on Github if they're quite long!

<details><summary>**Example Docker Compose File**</summary>
  
  ```
version: '2.2'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.13.1
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic

  es02:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.13.1
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data
    networks:
      - elastic

  es03:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.13.1
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic

  kib01:
    image: docker.elastic.co/kibana/kibana:7.13.1
    container_name: kib01
    ports:
      - 5601:5601
    environment:
      ELASTICSEARCH_URL: http://es01:9200
      ELASTICSEARCH_HOSTS: '["http://es01:9200","http://es02:9200","http://es03:9200"]'
    networks:
      - elastic

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge
  ```
  
  </details>
 
**4. Installing Logstash**
 
 -Future me please figure this out
  
**5. Viewing Collected Data**

You can view and mess around with data transferred by MetricBeat (or any other similar program) by heading to ```http://localhost:5601``` in your browser, and picking the Kibana Dashboard, then Discover. You can now search a long list of metrics, and hovering over one will give you the option to "Visualize" it, and it'll bc put on a graph!

From there, you can search other metrics, hover over them and click the plus symbol in the top right of the card to add them to your graph. You can also change chart types, and date ranges from the menu above the graph.

 </details>
 
 <details><summary>Downloading and Installing a complete pre-made ELK Stack setup</summary>
 
 ```
 Coming Soon
 ```
 </details>
 
 
 
 
 <details><summary>Optional Extras</summary>
 
 **1. Install and Run MetricBeat**

In ths guide, I will be using MetricBeat to monitor system metrics, such as CPU%, MEM% and so on. The commands in step 2 is explained in much more depth in **5.** for better understanding.

 | Step #  | Command* | Description |
| ------------- | ------------- | -------------|
| 1  | ```docker pull docker.elastic.co/beats/metricbeat:7.13.1``` | Download MetricBeat (7.13.1) |
| 2  | ```sudo docker run --network=elasticstack_networkName docker.elastic.co/beats/metricbeat:7.13.1 setup -E setup.kibana.host=kib01:5601 -E output.elasticsearch.hosts=["es01:9200"]```  | Run the initial setup. MetricBeat will pass to the server the names of all the indecies it tracks for the server to make a note of. This may take a moment. |
| 3  | ```sudo docker run --network=elasticstack_elastic docker.elastic.co/beats/metricbeat:7.13.1 metricbeat -e -E output.elasticsearch.hosts=["es01:9200"]```  | Run MetricBeat for real! Now metricbeat will be sending information to your server! |

\*Please Note: The commands at each step are all just single commands, but some may end up being across multiple lines here on Github if they're quite long!


**2. MetricBeat Installation Commands Explained**

The first command we ran when installing MetricBeat was ```sudo docker run --network=elasticstack_elastic docker.elastic.co/beats/metricbeat:7.13.1 setup -E setup.kibana.host=kib01:5601 -E output.elasticsearch.hosts=["es01:9200"]```.
| Command | Explanation |
| -------- | -------- |
| ```sudo docker run```  | Run the Docker Engine as superuser |
| ```--network=elasticstack_elastic``` | In order to communicate with the server, we need to specify the network name. For ELK, the network name is ``elasticstack_`` followed by the name specified in ```docker-compose.yml```. In this came, that name was "elastic", hence elasticstack_elastic |
| ```docker.elastic.co/beats/metricbeat:7.13.1 setup``` | Run metricbeat in Setup Mode |
| ```-E setup.kibana.host=kib01:5601``` | `-E` flags that we are about to specify the value of a variable, in this case `setup.kibana.host`. This value should be the name and port of the Kibana Container in `docker-compose.yml` (since we are running setup locally), which is `kib01:5601` in this case. |
| `-E output.elasticsearch.hosts=["es01:9200"]` | Specify the host URLs of the Elasticsearch Hosts as specified in `docker-compose.yml`. In this case, they are `es01:9200`, `es02:9200` and `es03:9200`, however in this case we have chosen to only use one. *If we weren't running MetricBeat on the same machine as the server, these URLs would have to be replaced with the Server's IP everywhere it appears (in both the MetricBeat and ELK setups), to ensure data can reach it from other machines on the network.
 </details>
 

## Demo Installation



## Sources

**1.** https://docs.docker.com/engine/install/centos/ 

**2.** https://docs.docker.com/compose/install/ 

**3.** https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html 

**4.** https://www.elastic.co/guide/en/beats/metricbeat/current/running-on-docker.html 
