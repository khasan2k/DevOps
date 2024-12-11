<p align="center">
 <img src="https://github.com/kamrulhassanbd/DevOps/blob/main/Images/Picture1.png" style="margin: 50px 0px" width="100%">
</p>

## What is Docker

 Docker is a software development tool and a virtualization technology that makes it easy to develop, deploy, and manage applications by using containers.

✨ Docker is an open-source Centralized platform designed to Create, deploy and run applications  
✨ Docker uses container on the host OS to run applications. It allows applications to use the same Linux kernel as a system on the host computer, rather than creating a whole virtual OS  
✨ We can install docker on any OS but docker engine runs natively on Linux distribution  
✨	Docker written in go language  
✨	Docker is a tool that performs OS Level Virtualization, also known as containerization  
✨	Before docker, many users face the problem that a particular code is running in the developer’s system but not in this system  
✨	Docker was first release in March 2013. It is developed by Solomon Hykes and Sebastion Pohl  
✨	Docker is set of Platform as a service that uses OS level virtualization whereas VMware uses Hardware Level Virtualization  

### Advantage of Docker:  
✨	No pre-allocation of RAM  
✨	CI efficiency > Docker enables you build a container image and use that same image across every step of deployment process  
✨	Less cost  
✨	Image is very light weight  
✨	It can run on physical H/W / Virtual H/W or on cloud  
✨	You can re-use the image  
✨	It takes very less time to run container  

## Container & Containerization
Container refers to a lightweight, stand-alone, executable package of a piece of software that contains all the libraries, configuration files, dependencies, and other necessary parts to operate the application.
Containerization is a system of intermodal freight transport using intermodal
containers (also called shipping containers and ISO containers). Containerization is also referred as "Container Stuffing" or "Container Loading", which is the process of unitization of cargoes in exports.

### Container Architecture:
 
### Virtualization:
Virtualization is technology that lets you create useful IT services using resources that are traditionally bound to hardware. It allows you to use a physical machine’s full capacity by distributing its capabilities among many users or environments.

Let’s Make Difference on Container & Virtualization:


Docker Architecture:

### ARCHITECTURE:
DOCKER CLIENT: it’s a way of interacting with docker (command -- > op)
DOCKER DAEMON: it manages all the docker components (images, cont, volumes, nlw)
DOCKER HOST: where we installed docker
DOCKER REGISTRY: it manages all the docker images on internet.
 
### INSTALLATION:
For Virtualization:
yum update -y
yum install yum-utils device-mapper-persistent-data lvm2 wget telnet vim -y
cd /etc/yum.repos.d/
wget https://download.docker.com/linux/centos/docker-ce.repo
yum repolist
yum install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl status docker
systemctl enable docker
systemctl is-enabled docker

### For AWS:

yum install docker -y
systemctl start docker
systemctl status docker

### Basic Commands:
docker version					: Show Docker Version
docker pull amazonlinux		 		: to download image
docker run -it --name cont1 amazonlinux 	: to create container
docker images					: to list images
docker start cont1					: to start cont1
docker stop cont1					: to stop cont1
docker kill cont1					: to stop immediately cont1
docker ps 						: to see running containers
docker ps -a						: to see all containers


### OS LEVEL VIRTUALIZATION:
NOTE: apt is package manager for ubuntu
Redhat: Yum
Ubuntu: Apt

without running apt update -y we can’t install packages

### WORKING:
docker pull ubuntu
docker run -it --name cont1 ubuntu
apt update -y
apt install git maven apache2 tree -y
touch file{1..5}

docker commit cont1 raham
docker run -it --name cont2 raham

DOCKERFILE:
it’s a way of creating images automatically.
we can reuse the docker file for multiple times.
in Dockerfile D will be Capital always.
Components inside the Dockerfile also Capital.

Dockerfile -- > Image -- > Container -- > 

COMPONENTS:
FROM			: to base image (gives OS)
RUN			: to execute Linux commands (image creation)
CMD			: to execute Linux commands (container creation)
ENTRYPOINT		: high priority than CMD
COPY 			: to copy local files to container
ADD 			: to copy Internet files to container
WORKDIR		: to go desired folder
LABEL 		: to attach our tags/labels
ENV			: variables which run inside container (image)
ARGS			: variables which run outside container(containers)
VOLUME		: used to create volume for container
EXPOSE		: used to give port number

EX: -1

FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y

Build: docker build -t netflix:v1 .
cont: docker run -it --name cont3 netflix:v1

EX: -2:
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
CMD apt install default-jre -y

Build: docker build -t netflix:v2 .
cont: docker run -it --name cont4 netflix:v2

EX-3:

FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp

EX-4:
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
WORKDIR /tmp
LABEL author rahamshaik

docker inspect cont7 
docker inspect cont7 | grep -i author

EX-5:
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
WORKDIR /tmp
LABEL author rahamshaik
ENV name vijay
ENV client swiggy

run commands inside container
echo $name
echo $client


EX-6:

FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
WORKDIR /tmp
LABEL author rahamshaik
ENV name vijay
ENV client swiggy
VOLUME ["/volume1"]
EXPOSE 8080

COMMANDS:
docker ps -a -q				: to list container ids
docker stop $(docker ps -a -q) 		: to stop all containers
docker rm $(docker ps -a -q) 		: to delete all containers
docker images -q				: to print image ids
docker rmi -f $(docker images -q) 	: to delete all images

HISTORY: 1  yum install docker -y
    2  service docker start
    3  service docker status
    4  docker pull ubuntu
    5  docker images
    6  docker run -it --name cont1 ubuntu
    7  docker ps -a
    8  docker attach cont1
    9  docker images
   10  docker ps -a
   11  docker commit cont1 raham
   12  docker images
   13  docker run -it --name cont2 raham
   14  vim Dockerfile
   15  docker build -t netflix:v1 .
   16  docker images
   17  docker run -it --name cont3 netflix:v1
   18  vim Dockerfile
   19  docker build -t netflix:v2 .
   20  docker run -it --name cont4 netflix:v2
   21  docker ps -a
   22  vim Dockerfile
   23  docker build -t netflix:v3 .
   24  ll
   25  vim index.html
   26  docker build -t netflix:v3 .
   27  docker run -it --name cont5 netflix:v3
   28  vim Dockerfile
   29  docker build -t netflix:v3 .
   30  docker run -it --name cont6 netflix:v1
   31  docker run -it --name cont7 netflix:v3
   32  docker inspect cont7
   33  docker inspect cont7 | grep -i author
   34  vim Dockerfile
   35  docker build -t netflix:v3 .
   36  docker run -it --name cont8 netflix:v3
   37  vim Dockerfile
   38  docker build -t netflix:v3 .
   39  docker run -it --name cont9 netflix:v3
   40  docker ps -a
   41  vim Dockerfile
   42  docker ps -a
   43  docker ps -a -q
   44  docker stop $(docker ps -a -q)
   45  docker ps -a
   46  docker rm $(docker ps -a -q)
   47  docker ps -a
   48  docker images
   49  docker images -q
   50  docker rmi -f $(docker images -q)
   51  history
==============================================================================================================

VOLUMES:
in docker, we use volumes to store the data.
volume is nothing but a folder inside a container.
we can share a volume from one container to another.
the volume contains the files which have data.
we can attach the single volume to multiple containers.
but at a time we can attach only one volume to one container.
volumes are decoupled (loosely attached)
if we delete the container volume will not be deleted.


METHOD-1:

DOCKER FILE:

FROM ubuntu
VOLUME ["/volume1"]

docker build -t netflix:v1 .
docker run -it --name cont1 netflix:v1
cd volume1
touch file{1..10}
ctrl p q

docker run -it --name cont2 --volumes-from cont1 --privileged=true ubuntu
cd volume1
ll


2. CLI:

docker run -it --name cont3 -v /volume2 ubuntu
cd volume2
touch java{1..10}
ctrl p q

docker run -it --name cont4 --volumes-from cont3 --privileged=true ubuntu
cd volume2
ll
ctrl p q

3. VOLUME MOUNTING:

volume commands:
docker volume create volume3
docker volume ls
docker volume inspect volume3

cd /var/lib/docker/volumes/volume3/_data
touch python{1..10}
ll
docker run -it --name cont5 --mount source=volume3,destination=/volume3 ubuntu


4. MOVING FILES FROM LOCAL TO CONTAINER:

create a connection and attach a volume for it

touch raham{1..10}
docker inspect cont6
docker volume inspect volume4
cp * /var/lib/docker/volumes/volume4/_data
docker attach cont6

5.
touch raham{1..10}
cp * /home/ec2-user
docker run -it --name cont12 -v /home/ec2-user:/abcd ubuntu

SYSTEM COMMANDS:
docker system df		: show docker components resource utilization
docker system df -v		: show docker components resource utilization individually
docker system prune	: to remove unused docker components

JENKINS SETUP:
docker run -it --name cont1 -p 8080:8080 jenkins/jenkins:lts

HISTORY:
  1  docker --version
    2  vim Dockerfile
    3  docker build -t netflix:v1 .
    4  docker images
    5  docker run -it --name cont1 netflix:v1
    6  docker ps -a
    7  docker run -it --name cont2 --volumes-from cont1 --privileged=true ubuntu
    8  docker run -it --name cont3 -v /volume2 ubuntu
    9  docker run -it --name --volumes-from cont3 --privileged=true cont4
   10  docker run -it --name cont4 --volumes-from cont3 --privileged=true ubuntu
   11  docker volume create volume3
   12  docker volume ls
   13  docker volume inspect volume3
   14  cd /var/lib/docker/volumes/volume3/_data
   15  touch python{1..10}
   16  ll
   17  docker run -it --name cont5 --mount source=volume3 destination=/volume3 ubuntu
   18  docker run -it --name cont5 --mount source=volume3, destination=/volume3 ubuntu
   19  docker run -it --name cont5 --mount source=volume3, dest=/volume3 ubuntu
   20* docker run -it --name cont5 --mount source=volume3,destination=/volume3 ubuntu cd
   21  cd
   22  docker volume create volume4
   23  docker volume ls
   24  docker volume inspect volume4
   25  cd /var/lib/docker/volumes/volume4/_data
   26  touch php{1..10}
   27  docker run -it --name cont6 --mount source=volume4,destination=/volume4 ubuntu
   28  ll
   29  cd
   30  touch raham{1..10}
   31  ll
   32  docker inspect cont6
   33  docker volume inspect volume4
   34  ll
   35  cp * /var/lib/docker/volumes/volume4/_data
   36  docker attach cont6
   37  docker volume inspect volume4
   38  ll
   39  docker run -it --name cont7 -v /root=/abc ubuntu
   40  docker run -it --name cont8 -v /root=abc ubuntu
   41  docker run -it --name cont7 -v /abc ubuntu
   42  docker run -it --name cont8 -v /abc ubuntu
   43  docker run -it --name cont9 -v /abc ubuntu
   44  ll
   45  cp * /home/ec2-user/
   46  cd /home/ec2-user/
   47  ll
   48  docker run -it --name cont10 -v /home/ec2-user/=abcd ubuntu
   49  docker run -it --name cont11 --volume /home/ec2-user/=abcd ubuntu
   50  docker run -it --name cont12 -v /home/ec2-user:abcd ubuntu
   51  docker run -it --name cont12 -v /home/ec2-user:/abcd ubuntu
   52  cd
   53  docker system
   54  docker system df
   55  docker system df -v
   56  docker volume create volume5
   57  docker volume create volume6
   58  docker system df -v
   59  docker pull centos
   60  docker pull amazonlinux
   61  docker system prune
   62  docker kill $(docker ps -a -q)
   63  docker create network raham
   64  docker network create raham
   65  docker network create raham2
   66  docker network create raham1
   67  docker system prune
   68  docker system events
   69  docker run -it --name cont1 -p 8080:8080 jenkins/jenkins:lts
   70  history
[root@ip-172-31-12-122 ~]#
====================================================================LINK: https://www.w3schools.com/howto/tryit.asp?filename=tryhow_css_form_icon

PROCESS: 
CODE -- > BUILD (DOCKER FILE) -- > IMAGE -- > CONTAINER -- > APP
http://3.7.248.36:81/

vim Dockerfile

FROM ubuntu
RUN apt update -y
RUN apt install apache2 -y
COPY index.html /var/www/html
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]

create index.html

docker build -t movies:v1 .
docker run -itd --name movies -p 81:80 movies:v1
public-ip:81 

Note: we can’t change port 80 (its default Apache port)

2. CHNAGE INDEX.HTML (MOVIES=TRAIN)

docker build -t train:v1 .
docker run -itd --name train -p 82:80 train:v1
public-ip:82

3. CHNAGE INDEX.HTML (TRAIN=DTH)

docker build -t dth:v1 .
docker run -itd --name dth -p 83:80 dth:v1
public-ip:83

4. CHNAGE INDEX.HTML (DTH=RECHARGE)

docker build -t recharge:v1 .
docker run -itd --name recharge -p 84:80 recharge:v1
public-ip:84

DOCKER COMPOSE:
	it’s a tool used to launch multiple containers.
	we can create multiple containers on single hosts.
	all of the services information we are going to write on a file.
	here we use docker-compose/compose file.
	it will be on yaml format.
	we use key-value pair (dictionary) (.yml or .yaml)
	used to manage multiple services with help of a file.

SETUP:
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
ls /usr/local/bin/
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
vim docker-compose.yml

version: '3.8'
services:
  movies:
    image: movies:v1
    ports:
      - "81:80"
  train:
    image: train:v1
    ports:
      - "82:80"
  dth:
    image: dth:v1
    ports:
      - "83:80"
  recharge:
    image: recharge:v1
    ports:
      - "84:80"

Note: remove all the containers


Commands:
docker-compose up -d		: to run all services
docker-compose down		: to remove all services
docker-compose stop		: to stop all services
docker-compose kill			: to kill all services
docker-compose rm			: to remove all services which is on stopped state
docker-compose start		: to start all services
docker-compose pause		: to pause all services
docker-compose unpause		: to unpause all services
docker-compose images		: to get all images managed by compose file
docker-compose ps -a		: to get all containers managed by compose file
docker-compose logs		: to get all logs managed by compose file
docker-compose scale dth=10	: to create 10 containers of dth
docker-compose top		: to get all process managed by containers on compose file

DEFAULT FILE CHANGE:
mv docker-compose.yml raham.yml
docker-compose stop
Supported filenames: docker-compose.yml, docker-compose.yaml, compose.yml, compose.yaml
docker-compose -f raham.yml stop

=================================

DOCKER HUB:
	It’s a central platform used to store docker images.
	image we create on host then we will push them to dockerhub.
	Once image is available on dockerhub we can use in any server.
	we use repos to store images.
	repo types: 1. public 2. private repo

ACCOUNT:
build a docker image with doker file.
docker login: username and password

docker tag image:v1 username/reponame
docker push username/reponame

DOCKER SWARM:
	High availability: deploying app on more than one server
	means using the cluster.
	docker swarm is a container orchestration tool.
	it is used to manage multiple containers on multiple nodes.
	each node will have a copy of single container.
	here we have manager and worker nodes.
	manager node will create containers and send to worker nodes.
	worker nodes will take the container and manage them.
	manager node communicates with worker node by using token.

SETUP:
1. create 3 servers (1=manager 2=worker) (all traffic is must)
yum install docker -y
systemctl start docker
systemctl status docker

2. set the hostnames:
hostnamectl set-hostname manager/worker-1/worker-2

3. generate and copy token:
manager node: docker swarm init
copy the token to all worker nodes

SERVICES:
its a way of exposing the application in docker.
with services we can create multiple copies of same container.
with services we can distribute the containers to all servers.

docker service create --name movies --replicas=3 --publish 81:80 jayaprakashsairam05/movies:latest

docker service ls			: to list services
docker service ps train		: to list containers for train
docker service inspect train	: to get complete info of train service
docker service scale train=10	: to scale the services
docker service scale train=6	: to scale the services
docker service rollback train	: to go back to previous state
docker service rm train		: to delete the train services

SELF HEALING: automatically recreates container itself.

CLUSTER LEVEL ACTIVITES:
docker swarm leave (worker node)	: to down the worker node
docker node rm id (manager node): to remove the node permanently
docker swarm join-token  manager: to regenerate the token


Note: to join the node on cluster use the previous token
we can delete running worker node directly
we need to stop and then delete it


HISTORY:
   1  docker imgages
    2  docker images
    3  docker swarm init
    4  docker node ls
    5  docker run -itd --name cont1 jayaprakashsairam05/movies:latest
    6  docker ps -a
    7  docker kill cont1
    8  docker rm cont1
    9  docker service create --name movies --replicas=3 --publish 81:80 jayaprakashsairam05/movies:latest
   10  docker ps -a
   11  docker service create --name train --replicas=6 --publish 82:80 jayaprakashsairam05/train:latest
   12  docker ps -a
   13  docker service ls
   14  docker ps -a
   15  docker service ls
   16  docker service ps train
   17  docker service inspect train
   18  docker service ls
   19  docker service scale train=10
   20  docker service ls
   21  docker service ps train
   22  docker service scale train=6
   23  docker service rollback train
   24  docker service rm train
   25  docker service ls
   26  docker ps -a
   27  docker kill 3d6965f5a234
   28  docker ps -a
   29  docker kill 159af68bfd56
   30  docker ps -a
   31  docker node ls
   32  docker node rm c2ldr2jfatw9p8s4uqzrf329h
   33  docker node ls
   34  docker node rm uhru8isgfco57di11azvbk2c9
   35  docker node ls
   36  docker node rm 3tm927qkvrt8sdr7sz7gprs7t
   37  docker node ls
   38  docker node rm 3tm927qkvrt8sdr7sz7gprs7t
   39  docker node ls
   40  docker swarm joint-token manager
   41  docker swarm join-token manager
   42  history
[root@manager ~]#

=============================================
PORTAINER:
	it is a container organizer, designed to make tasks easier, whether they are clustered or not. 
	abel to connect multiple clusters, access the containers, migrate stacks between clusters
	it is not a testing environment mainly used for production routines in large companies.
	Portainer consists of two elements, the Portainer Server and the Portainer Agent. 
	Both elements run as lightweight Docker containers on a Docker engine

SETUP:
Must have swarm mode and all ports enable with docker engine
curl -L https://downloads.portainer.io/ce2-16/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
 docker ps
public-ip of swamr master:9000

DOCKER NETWORKING:
Docker networks are used to make a communication between the multiple containers that are running on same or different docker hosts. We have different types of docker networks.
	Bridge Network
	Host Network
	None network
	Overlay network

	BRIDGE NETWORK: It is a default network that container will communicate with each other within the same host.

	HOST NETWORK: When you Want your container IP and ec2 instance IP same then you use host network

	NONE NETWORK: When you don’t Want The container to get exposed to the world, we use none network. It will not provide any network to our container.

	OVERLAY NETWORK: Used to communicate containers with each other across the multiple docker hosts.


To create a network: docker network create network_name
To see the list: docker network ls
To delete a network: docker network rm network_name
To inspect: docker network inspect network_name
To connect a container to the network: docker network connect network_name container_id/name
apt install iputils-ping -y : command to install ping checks
To disconnect from the container: docker network disconnect network_name container_name
To prune: docker network prune


RESOURCE MANAGEMENT:
continers are going to use host resources. (cpu, mem and ram)
we need to limit that resoure utilization.
by default containers are not having limts.


docker run -itd --name cont3 --cpus="0.5" --memory="500mb" ubuntu 
docker inspect cont3
docker stats
docker top cont3


 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 



 

 

 

