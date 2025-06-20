# How to Install Minikube for Kubernetes on Ubuntu 24.04
## Step 1: Install Dependencies for Minikube
First, you need to update the system package list and install necessary dependencies like curl, wget, apt-transport-https, and VirtualBox.
```
sudo apt-get update -y 
sudo apt upgrade -y
sudo apt install -y curl apt-transport-https
```
 ## Install Docker Engine on Ubuntu
 ```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER && newgrp docker
 ```
 
## Step 2: Install Kubectl in Ubuntu
Next, you need to install kubectl, which is a command-line tool for interacting with Kubernetes clusters.
```
curl -LO "https://dl.k8s.io/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

## Step 3: Install Minikube in Ubuntu
Now you can download and install the latest version of Minikube using the following commands.
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Now, you can start Minikube, which will download and set up the required components.

```
minikube start --driver=docker
```
To check if Minikube is running correctly, use:
```
kubectl get nodes
```
## Step 4: Managing Minikube in Ubuntu
Here are some basic commands to manage Minikube:

To stop the Minikube cluster, run:
```
minikube stop
```
To delete the Minikube cluster, run:
```
minikube delete
```
Minikube provides a web-based dashboard, you can access it by running:
```
minikube dashboard
```
