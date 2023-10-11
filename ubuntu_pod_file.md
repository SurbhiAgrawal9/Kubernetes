How to Deploying on Kubernetes

# Init containers
# First, create a working directory and navigate in:
1)	ls
2)	rm -rf ubuntu_pod

3)	 mkdir ubuntu_pod

4)	ls

5)	 cd ubuntu_pod
# Create a yaml file named ubuntu_pod.yaml, put in the following:
6)	nano ubuntu_pod.yaml

   ```js
apiVersion: v1
kind: Pod
metadata:
name: ubuntu
# namespace: development
spec:
containers:
- name: ubuntu
image: ubuntu
command:
- sleep
- infinity
initContainers:
- name: install
image: busybox:1.28
command:
- wget
- "-O"
- "/work-dir/index.html"
- http://info.cern.ch
volumeMounts:
- name: workdir
mountPath: "/work-dir"
dnsPolicy: Default
volumes:
- name: workdir
emptyDir: {}
```
#  save the file and exit
# Create the Ubuntu pod

7)	kubectl apply -f   ubuntu_pod.yaml

#  Check Pod
8)	 kubectl get pod

#  Get a shell for the pod by executing the following command:

9)	 kubectl exec --stdin --tty ubuntu -- /bin/bash

10)	ls

11)	 apt update && apt upgrade

12)	   apt-get install sudo
    
#  Check the sudo command path
13)	  which sudo

14)	sudo apt install  nano

15)	   exit

#  Here is the syntax to copy a file from local to pod  copy ifconfig commad run from local that means ubuntu@1.1.1.107

9)	kubectl cp /usr/sbin/ifconfig ubuntu:/usr/sbin/ifconfig -c ubuntu

#again enter in to the pod  
10)	   kubectl exec --stdin --tty ubuntu -- /bin/bash
11)	   ls

# now run ifconfig
12)	   ifconfig

# check the mysql install or not if not install from the Link which is given below

13)	   which mysql
https://linuxhint.com/install-mysql-on-ubuntu-22-04/

14)	  which python3

/usr/bin/python3
this command is install  the python mysql connector
15)	  sudo apt-get install python3-mysql.connector

16)	  cat > prog
import mysql.connector
print("hello");
^D
17)	python3 prog
ex:-
root@ubuntu:/home/python_mysql#
root@ubuntu:/home/python_mysql# python3 prog
hello
root@ubuntu:/home/python_mysql#
root@ubuntu:/home/python_mysql#




