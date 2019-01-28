#Running docker-compose azure

Docker Compose is a tool to help orchestrate the running of docker containers. 

To install docker on azure follow the Digital Ocean instructions

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-centos-7

Note if you have preiviously installed docker using thee centos command line (yum install docker), first remove docker before performing the digital ocean tutorial
i.e. frstt find out which docker packages installed   
sudo yum list installed | grep docker
sudo yum remove docker-client
sudo yum remove docker-common

To install docker-compose follow the Digital Ocean instructions

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-centos-7

