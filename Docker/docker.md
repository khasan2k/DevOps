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
Containerization is a system of intermodal freight transport using intermodal containers (also called shipping containers and ISO containers). Containerization is also referred as "Container Stuffing" or "Container Loading", which is the process of unitization of cargoes in exports.

### Container Architecture:
 <p align="center">
 <img src="https://github.com/kamrulhassanbd/DevOps/blob/main/Images/Conatainer_architecture.png" style="margin: 50px 0px">
</p>

### Virtualization:
Virtualization is technology that lets you create useful IT services using resources that are traditionally bound to hardware. It allows you to use a physical machine’s full capacity by distributing its capabilities among many users or environments.

Let’s Make Difference on Container & Virtualization:

<p align="center">
 <img src="https://github.com/kamrulhassanbd/DevOps/blob/main/Images/vmsvscontainer.jpg" style="margin: 50px 0px">
 <img src="https://github.com/kamrulhassanbd/DevOps/blob/main/Images/vm_vs_cont.jpg" style="margin: 50px 0px">
</p>

### Docker Architecture:
<p align="center">
<img src="https://github.com/kamrulhassanbd/DevOps/blob/main/Images/docker_architecture.png" style="margin: 50px 0px">
</p>

### ARCHITECTURE:
**DOCKER CLIENT:** It’s a way of interacting with docker (command -- > op)   
**DOCKER DAEMON:** It manages all the docker components (images, cont, volumes, nlw)   
**DOCKER HOST:** Where we installed docker   
**DOCKER REGISTRY:** It manages all the docker images on internet.   
 
### INSTALLATION:

#### Using docker convenience script
https://docs.docker.com/engine/install/

#### For Virtualization:   
```
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
```

#### For AWS:
#### Install Docker in Redhat
```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
sudo systemctl start docker
sudo groupadd -f docker
sudo usermod -aG docker $USER
sudo service docker start
```
#### Uninstall Docker
```
sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine \
                  podman \
                  runc
```

### Container Commands:
| Command | Description |
| --- | --- |
| docker version | **➺ Show Docker Version** |
| docker pull amazonlinux | **➺ to download image** |
| docker run --name cont1 amazonlinux | **➺ to create container** |
| docker run -p 8080:80 <image> | **➺ Publish container port 80 to host port 8080** |
| docker run -d --name cont1 amazonlinux | **➺ Run a container in the background** |
| docker run -v <host>:<container> <image> | **➺ Mount a host directory to a container** |
| docker start cont1 | **➺ to start cont1** |
| docker stop cont1 | **➺ to stop cont1** |
| docker kill cont1 |  **➺ to stop immediately cont1** |
| docker ps | **➺ to see running containers** |
| docker ps -a | **➺ to see all containers** |
| docker ps -a -q | **➺ to list container ids** |
| docker stop $(docker ps -a -q) | **➺ to stop all containers** |
| docker rm $(docker ps -a -q) | **➺ to delete all containers** |
| docker logs <container_name> | **➺ Fetch the logs of a container** |
| docker logs -f <container_name> | **➺ Fetch and follow the logs of a container** |
| docker cp cl:/usr/share/nginx/html/container.txt . | **➺ Copy from container to local system** |

### Executing commands in a container
| Command | Description |
| --- | --- |
| `docker exec <container_name> <command>` | **➺ Execute a command in a running container |
| `docker exec -it <container_name> bash` | **➺ Open a shell in a running container** |

### Image commands

| Command | Description |
| --- | --- |
| `docker images` | **➺ to list images**    |
| `docker build -t <image> .` | **➺ to print image ids** |
| `docker images -q` | **➺ Build a new image from the Dockerfile in the current directory and tag it**  |
| `docker rmi -f $(docker images -q)` 	 |  **➺ to delete all images**  |
| `docker image prune` | **➺ Delete unused images**   |


| Command | Description |
| --- | --- |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
				                   
### Container registry commands   
`docker login` **➺Login to Docker Hub**   
`docker login <server>` **➺Login to another container registry**   
`docker logout` **➺Logout of Docker Hub**   
`docker logout <server>` **➺Logout of another container registry**   
`docker push <image>` **➺Upload an image to a registry**   
`docker pull <image>` **➺Download an image from a registry**   
`docker search <image>` **➺Search Docker Hub for images**   
 
### Transfer docker image
`docker commit container_name ym-class:99`   
`docker save ym-class:99 | gzip > "ym-class.tar.gz"`   
`docker load -i ym-class.tar.gz`  

### DEFAULT FILE CHANGE:
```
Supported filenames: docker-compose.yml, docker-compose.yaml, compose.yml, compose.yaml   
docker-compose -f raham.yml stop   
```

### DOCKERFILE:

✨ It’s a way of creating images automatically.   
✨ We can reuse the docker file for multiple times.   
✨ In Dockerfile D will be Capital always.   
✨ Components inside the Dockerfile also Capital.   

**Dockerfile -- > Image -- > Container -- >**

### COMPONENTS:

**FROM**		: to base image (gives OS)   
**RUN**		: to execute Linux commands (image creation)   
**CMD**			: to execute Linux commands (container creation)   
**ENTRYPOINT**		: high priority than CMD   
**COPY** 			: to copy local files to container   
**ADD** 			: to copy Internet files to container   
**WORKDIR**		: to go desired folder   
**LABEL** 		: to attach our tags/labels   
**ENV**			: variables which run inside container (image)   
**ARGS**			: variables which run outside container(containers)   
**VOLUME**		: used to create volume for container   
**EXPOSE**		: used to give port number   

### Instructions

`FROM <image>` **➺Set the base image**   
`FROM <image> AS <name>` **➺ Set the base image and name the build stage**   
`RUN <command>` **➺ Execute a command as part of the build process**   
`RUN ["exec", "param1", "param2"]` **➺ Execute a command as part of the build process**   
`CMD ["exec", "param1", "param2"]` **➺ Execute a command when the container starts**   
`ENTRYPOINT ["exec", "param1"]` **➺ Configure the container to run as an executable**   
`ENV <key>=<value>` **➺ Set an environment variable**   
`EXPOSE <port>` **➺ Expose a port**   
`COPY <src> <dest>` **➺ Copy files from source to destination**   
`COPY --from=<name>` **➺ <src> <dest> Copy files from a build stage to destination**   
`WORKDIR <path>` **➺ Set the working directory**   
`VOLUME <path>` **➺ Create a mount point**   
`USER <user>` **➺ Set the user**   
`ARG <name>` **➺ Define a build argument**   
`ARG <name>=<default>` **➺ Define a build argument with a default value**   
`LABEL <key>=<value>` **➺ Set a metadata label**   
`HEALTHCHECK <command>` **➺ Set a healthcheck command**   

#### EX: -1

```
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
```
**Build:** `docker build -t netflix:v1 .`   
**Cont:** `docker run -it --name cont3 netflix:v1`

#### EX: -2:

```
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
CMD apt install default-jre -y
```
**Build:** `docker build -t netflix:v2 .`   
**Cont:** `docker run -it --name cont4 netflix:v2`   

#### EX-3:

```
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
```

#### EX-4:

```
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
WORKDIR /tmp
LABEL author rahamshaik
```
docker inspect cont7   
docker inspect cont7 | grep -i author   

#### EX-5:

```
FROM ubuntu
RUN apt update -y
RUN apt install git maven tree apache2 -y
COPY index.html /tmp
ADD https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz /tmp
WORKDIR /tmp
LABEL author rahamshaik
ENV name vijay
ENV client swiggy
```
**run commands inside container**   
echo $name   
echo $client


#### EX-6:

```
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
```

=====================================================================================
## VOLUMES:

✨ In docker, we use volumes to store the data.   
✨ Volume is nothing but a folder inside a container.   
✨ We can share a volume from one container to another.   
✨ The volume contains the files which have data.   
✨ We can attach the single volume to multiple containers.   
✨ But at a time we can attach only one volume to one container.   
✨ Volumes are decoupled (loosely attached)   
✨ If we delete the container volume will not be deleted.   


### METHOD:

#### 1. DOCKER FILE:

```
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
```

#### 2. CLI:

```
docker run -it --name cont3 -v /volume2 ubuntu
cd volume2
touch java{1..10}
ctrl p q

docker run -it --name cont4 --volumes-from cont3 --privileged=true ubuntu
cd volume2
ll
ctrl p q
```

#### 3. VOLUME MOUNTING:

```
volume commands:
docker volume create volume3
docker volume ls
docker volume inspect volume3

cd /var/lib/docker/volumes/volume3/_data
touch python{1..10}
ll
docker run -it --name cont5 --mount source=volume3,destination=/volume3 ubuntu
```

#### 4. MOVING FILES FROM LOCAL TO CONTAINER:

```
create a connection and attach a volume for it

touch raham{1..10}
docker inspect cont6
docker volume inspect volume4
cp * /var/lib/docker/volumes/volume4/_data
docker attach cont6
```
#### 5. 

```
touch raham{1..10}
cp * /home/ec2-user
docker run -it --name cont12 -v /home/ec2-user:/abcd ubuntu
```

### SYSTEM COMMANDS:
`docker system df`		: show docker components resource utilization   
`docker system df -v`		: show docker components resource utilization individually   
`docker system prune`	: to remove unused docker components   

## DOCKER COMPOSE:
✨	It’s a tool used to launch multiple containers.   
✨	We can create multiple containers on single hosts.   
✨	All of the services information we are going to write on a file.   
✨	Here we use docker-compose/compose file.   
✨	It will be on yaml format.   
✨	We use key-value pair (dictionary) (.yml or .yaml)   
✨	Used to manage multiple services with help of a file.   

### SETUP:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
ls /usr/local/bin/
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```
`vim docker-compose.yml`
```
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
```

**Note:** remove all the containers

### Commands:

`docker compose up` **➺ Create and start containers**    
`docker compose up -d` **➺ Create and start containers in background**    
`docker compose up --build` **➺ Rebuild images before starting containers**    
`docker compose stop` **➺ Stop services**    
`docker compose down` **➺ Stop and remove containers and networks**    
`docker-compose kill`			**➺ to kill all services**    
`docker-compose rm`		**➺ to remove all services which is on stopped state**    
`docker-compose start`		**➺ to start all services**    
`docker-compose pause`		**➺ to pause all services**    
`docker-compose unpause`		**➺ to unpause all services**    
`docker-compose images`		**➺ to get all images managed by compose file**    
`docker-compose ps -a`		**➺ to get all containers managed by compose file**    
`docker compose ps` **➺ List running containers**    
`docker compose logs` **➺ View the logs of all containers**    
`docker compose logs <service>` **➺ View the logs of a specific service**    
`docker compose logs -f` **➺ View and follow the logs**    
`docker compose pull` **➺ Pull the latest images**    
`docker compose build` **➺ Build or rebuild services**    
`docker compose build --pull` **➺ Pull latest images before building**    
`docker-compose scale dth=10`	**➺ to create 10 containers of dth**    
`docker-compose top`		**➺ to get all process managed by containers on compose file**    

=================================

## DOCKER HUB:
	It’s a central platform used to store docker images.
	image we create on host then we will push them to dockerhub.
	Once image is available on dockerhub we can use in any server.
	we use repos to store images.
	repo types: 1. public 2. private repo

### ACCOUNT:
build a docker image with doker file.   
docker login: username and password   

`docker tag image:v1 username/reponame`
`docker push username/reponame`

## DOCKER SWARM:
	High availability: deploying app on more than one server
	means using the cluster.
	docker swarm is a container orchestration tool.
	it is used to manage multiple containers on multiple nodes.
	each node will have a copy of single container.
	here we have manager and worker nodes.
	manager node will create containers and send to worker nodes.
	worker nodes will take the container and manage them.
	manager node communicates with worker node by using token.

### SETUP:
#### 1. create 3 servers (1=manager 2=worker) (all traffic is must)
```
yum install docker -y
systemctl start docker
systemctl status docker
```
#### 2. set the hostnames:
`hostnamectl set-hostname manager/worker-1/worker-2`

#### 3. generate and copy token:
manager node: `docker swarm init`
copy the token to all worker nodes

### SERVICES:
its a way of exposing the application in docker.   
with services we can create multiple copies of same container.   
with services we can distribute the containers to all servers.   

`docker service create --name movies --replicas=3 --publish 81:80 jayaprakashsairam05/movies:latest`   

`docker service ls`			: to list services   
`docker service ps train`		: to list containers for train   
`docker service inspect train`	: to get complete info of train service   
`docker service scale train=10`	: to scale the services   
`docker service scale train=6`	: to scale the services   
`docker service rollback train`	: to go back to previous state   
`docker service rm train`		: to delete the train services   

**SELF HEALING:** automatically recreates container itself.   

### CLUSTER LEVEL ACTIVITES:   
`docker swarm leave` (worker node)`	: to down the worker node   
docker node rm id` (manager node)`: to remove the node permanently   
docker swarm join-token manager: to regenerate the token   


Note: to join the node on cluster use the previous token   
we can delete running worker node directly   
we need to stop and then delete it    

=============================================
## PORTAINER:
	it is a container organizer, designed to make tasks easier, whether they are clustered or not.    
	abel to connect multiple clusters, access the containers, migrate stacks between clusters   
	it is not a testing environment mainly used for production routines in large companies.   
	Portainer consists of two elements, the Portainer Server and the Portainer Agent.   
	Both elements run as lightweight Docker containers on a Docker engine   

### SETUP:
Must have swarm mode and all ports enable with docker engine   
```
curl -L https://downloads.portainer.io/ce2-16/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
docker ps
public-ip of swamr master:9000
```
### DOCKER NETWORKING:
Docker networks are used to make a communication between the multiple containers that are running on same or different docker hosts. We have different types of docker networks.   
	Bridge Network   
	Host Network   
	None network   
	Overlay network   

	**BRIDGE NETWORK:** It is a default network that container will communicate with each other within the same host.   

	**HOST NETWORK:** When you Want your container IP and ec2 instance IP same then you use host network   

	**NONE NETWORK:** When you don’t Want The container to get exposed to the world, we use none network. It will not provide any network to our container.   

	**OVERLAY NETWORK:** Used to communicate containers with each other across the multiple docker hosts.   


To create a network: docker network create network_name   
To see the list: docker network ls   
To delete a network: docker network rm network_name   
To inspect: docker network inspect network_name   
To connect a container to the network: docker network connect network_name container_id/name   
apt install iputils-ping -y : command to install ping checks   
To disconnect from the container: docker network disconnect network_name container_name   
To prune: docker network prune   


### RESOURCE MANAGEMENT:
Continers are going to use host resources. (cpu, mem and ram)   
we need to limit that resoure utilization.   
by default containers are not having limts.   


docker run -itd --name cont3 --cpus="0.5" --memory="500mb" ubuntu 
docker inspect cont3
docker stats
docker top cont3
