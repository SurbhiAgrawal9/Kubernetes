# Kubernetes

Introduction

Kubernetes is an open source platform for managing container technologies such as Docker.

Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
Docker lets you create application containers from a pre-configured image. Kubernetes provides the next step, allowing you to balance loads between the containers and run multiple containers across multiple systems.

How to Install Kubernetes on Ubuntu 20.04
some command prefer from ==> https://phoenixnap.com/kb/install-kubernetes-on-ubuntu


Prerequisites:-

1)	Hardware Requirement:    
                For Master: 
               RAM: 2 GB 
               CPU: 2 Cores  
               For Slave: 
               RAM: 1 GB 
              CPU: 1 Core 
2)	Software Requirement:   
              2 or more Ubuntu 18.04 servers Access to a sudo or root privileged user account on       each device 
            The apt package manager. 
 
3)	 Machine Id should be different on each machine

 Set up Docker  on  Master Node

Installation Procedure: -
1)	Sudo su
2)	apt-get update
3)	apt-get upgrade
4)	sudo apt install docker.io -y
5)	sudo systemctl enable docker
6)	sudo systemctl status docker
 Note:- Start Docker if it is not running:     sudo systemctl start docker
       Install Kubernetes on Master Node

1)	curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo tee /usr/share/keyrings/kubernetes.gpg

2)	echo "deb [arch=amd64 signed-by=/usr/share/keyrings/kubernetes.gpg] http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list

3)	sudo apt install kubeadm kubelet kubectl

4)	sudo systemctl status kubeadm kubelet kubectl

5)	sudo apt-mark hold kubeadm kubelet kubectl

6)	kubeadm version

      Deploy Kubernetes

7)	sudo swapoff -a

8)	sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

9)	sudo nano /etc/modules-load.d/containerd.conf

 Add the following two lines:
overlay
br_netfilter

10)	sudo modprobe overlay 
11)	 sudo modprobe br_netfilter
12)	sudo nano /etc/sysctl.d/kubernetes.conf

   Add the following lines:
      net.bridge.bridge-nf-call-ip6tables = 1
     net.bridge.bridge-nf-call-iptables = 1
     net.ipv4.ip_forward = 1
13)	sudo sysctl –system
         Assign Unique Hostname for Each Server Node

14)	sudo hostnamectl set-hostname master-node
ex:-   sudo hostnamectl set-hostname Master-Ubuntu
15)	nano /etc/hosts
   Add the following lines:   
 1.1.1.104       Master-Ubuntu (server node Ip address & name)
1.1.1.107       worker01(worker node Ip address & name)
         

Initialize Kubernetes on Master Node
1)	sudo nano /etc/default/kubelet

Add the following line to the file
KUBELET_EXTRA_ARGS="--cgroup-driver=cgroupfs"
2)	systemctl daemon-reload
3)	systemctl restart kubelet
4)	sudo nano /etc/docker/daemon.json
Append the following configuration block:

{
      "exec-opts": ["native.cgroupdriver=systemd"],
      "log-driver": "json-file",
      "log-opts": {
      "max-size": "100m"
   },
       "storage-driver": "overlay2"
       }



5)	systemctl daemon-reload
6)	systemctl restart docker
7)	sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
Add the line below to the file:
Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"
8)	systemctl daemon-reload
9)	systemctl restart kubelet
10)	kubeadm init --apiserver-advertise-address=1.1.1.104 --pod-network-cidr=10.244.0.0/16
(1.1.1.104  - this is a master node ip address)
prefer from  github Link ==>   https://github.com/viveksinghggits/cluster-setup
                           Youtube Link   https://youtu.be/shGDqYenoos
11)	mkdir -p $HOME/.kube
12)	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
13)	sudo chown $(id -u):$(id -g) $HOME/.kube/config
14)	export KUBECONFIG=/etc/kubernetes/admin.conf


 Deploy Pod Network to Cluster
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

Run this command on Master After join command in worker node 
kubectl get nodes







