## Table of contents
- [Docker](#docker)

- [Installation Docker on Linux](#installation-docker-on-linux)

- [Docker Basics](#docker-basics)

- [Docker daemon and client:](#docker-daemon-and-client)

- [Docker Client (You):](#docker-client-you)

- [The layers concept.](#the-layers-concept)

- [Docker Volume:](#docker-volume)

- [Data Persistence:](#data-persistence)

- [Shared Storage:](#shared-storage)

- [Container-to-Container Communication:](#container-to-container-communication)

- [Syntax for Using Volumes:](#syntax-for-using-volumes)

- [Docker networking:](#docker-networking)

- [Custom Bridge Networks:](#custom-bridge-networks)

- [Host Network:](#host-network)

- [Overlay Network](#overlay-network)

- [Service Discovery and DNS:](#service-discovery-and-dns)

- [Network Inspect and Troubleshooting:](#network-inspect-and-troubleshooting)

- [Docker networking (DNS Enable):](#docker-networking-dns-enable)

- [Docker Registry:](#docker-registry)

- [Registry Server](#registry-server)

- [Docker Repository](#docker-repository)









# Docker
Docker is a tool that allows developers, sys-admins etc. to easily deploy their applications in a sandbox (called containers) to run on the host operating system that is Linux. The key benefit of Docker is that it allows users to package an application with all of its dependencies into a standardized unit for software development. 
Here's a breakdown of the key concepts in Docker:
* 1: Containerization:
Containers: Think of a container as a lightweight, standalone, and executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, and system tools. Containers are isolated from the underlying system and from other containers, ensuring consistency and reproducibility.

* 2: Docker Images:
A Docker image is a snapshot or template of a container. It contains the application code and all its dependencies, organized in layers. Images serve as the blueprint for creating containers.

* 3:Dockerfile: 
A Dockerfile is a script that contains instructions for building a Docker image. It specifies the base image, sets up the environment, and defines the commands to run.

##  Installation Docker on Linux
## Prerequisites:
```Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.3 LTS
Release:	22.04
Codename:	jammy
``````

### Step 1: Install Docker using the following command:

 ```
 sudo apt install docker.io
 ```
To check version:
```
Sudo docker –version
```
Output:  
Docker version 20.10.21, build 20.10.21-0ubuntu1~22.04.3  
To check the status of docker we have:  
```
sudo systemctl status docker  
```
● docker.service - Docker Application Container Engine  
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset:   enabled)  
     Active: active (running) since Mon 2023-11-20 10:09:58 IST; 1 week 0 days ago  
TriggeredBy: ● docker.socket  
       Docs: https://docs.docker.com  
   Main PID: 1284 (dockerd)  
      Tasks: 27  
     Memory: 42.8M  
        CPU: 31min 57.398s  

If your docker status is not active:
```
sudo systemctl enable –now docker
```
## Docker Basics    

To pull image from dockerhub use the following command:-  
```
sudo docker pull hello-world  
```
Output:   
Using default tag: latest  
latest: Pulling from library/hello-world  
719385e32844: Already exists   
Digest: sha256:c79d06dfdfd3d3eb04cafd0dc2bacab0992ebc243e083cabe208bac4dd7759e0  
Status: Downloaded newer image for hello-world:latest  
docker.io/library/hello-world:latest  

**To check the list of images:-**
```
sudo docker image ls
```
**Output:**

            REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
        s hello-world         latest    9c7a54a9a43c   6 months ago   13.3kB

To run image use this following command:
```
sudo docker run hello-world
```
Output:  
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 
## Docker daemon and client: 
The Docker daemon and client are like a team that helps you work with containers.:
Docker Daemon (Server):
Analogy: Think of the Docker daemon as a powerful worker or assistant in the background.
Role: The daemon does the heavy lifting. It manages all the containers, images, and other Docker stuff on your computer.
Job: It's like a handyman who takes care of building, running, and stopping containers. It's always there, ready to do the work for you.
## Docker Client (You):
Analogy: Imagine the Docker client as your friendly manager or boss.
Role: The client is your interface, your way of telling the daemon what to do.
Job: You, as the client, give commands (like 'build this container' or 'run that image') to the daemon. The client talks to the daemon and makes things happen.

## The layers concept.
When you build a Docker image using a Dockerfile, each instruction in the Dockerfile creates a layer in the final image. Docker layers are crucial for optimizing image size, improving build speed, and managing image updates efficiently.
Base Image Layer:Every Docker image starts with a base image layer. This layer contains the operating system and essential components.It's the foundation upon which you build your custom image.
```mkdir kemo
sumit@sumit:~$ cd kemo
sumit@sumit:~/kemo$ touch dockerfile
sumit@sumit:~/kemo$ vi dockerfile
```

###All commands used in layered concept

```FROM ubuntu:23.04
LABEL name="sumit chaubey"
LABEL email="sumitchaubey065@gmail.com"
ENV NAME saket
ENV PASS password
RUN pwd>/tmp/1stpwd.txt
RUN cd /tmp/
RUN pwd>/tmp/2ndpwd.txt
WORKDIR /tmp
RUN pwd>/tmp/3rdpwd.txt
RUN apt-get update && apt-get install -y openssh-server && apt-get update && apt-get install -y python
RUN useradd -d /home/sumit -g root -G sudo -m -p $(echo "$PASS" | openssl passwd -1 -stdin) $NAME
RUN whoami > /tmp/1stwhoami.txt
USER $NAME
RUN whoami > /tmp/2ndwhoami.txt
RUN mkdir -p /tmp/project
COPY dockerfile /tmp/project/
CMD ["sh"]
```

```
sumit@sumit:~/kemo$ sudo docker image build -t myoperation:01 .
[sudo] password for sumit: 
```
```
Sending build context to Docker daemon  2.048kB
Step 1/1 : FROM ubuntu:23.04
 ---> 639282825872
Successfully built 639282825872
Successfully tagged myoperation:01
sumit@sumit:~/kemo$ sudo docker image ls
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
myterminal               00        c315705c381a   15 hours ago   125MB
myterminal               02        8beba2d29e49   16 hours ago   125MB
my_ubuntu_t_g            latest    8627ebf02da2   7 days ago     198MB
tomcat                   latest    fa51ad4dee22   3 weeks ago    460MB
sumitchaubey065/tomcat   latest    fa51ad4dee22   3 weeks ago    460MB
myterminal               01        e4c58958181a   7 weeks ago    77.8MB
ubuntu                   latest    e4c58958181a   7 weeks ago    77.8MB
myoperation              01        639282825872   7 weeks ago    70.3MB
ubuntu                   23.04     639282825872   7 weeks ago    70.3MB
```

```
sumit@sumit:~/kemo$ sudo docker container run -it myoperation:01
```
```
root@48a39072b11c:/# exit
exit
sumit@sumit:~/kemo$ tree
```
Command 'tree' not found, but can be installed with:
sudo snap install tree  # version 1.8.0+pkg-3fd6, or
sudo apt  install tree  # version 2.0.2-1
See 'snap info tree' for additional versions.
```
sumit@sumit:~/kemo$ vi dockerfile
ENV NAME saket
ENV PASS password
```
```
sumit@sumit:~/kemo$ sudo docker image build -t myoperation:02 .
```

Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM ubuntu:23.04
 ---> 639282825872
Step 2/2 : RUN apt-get update && apt-get install -y tree
 ---> Running in 10090c78c832
Get:1 http://security.ubuntu.com/ubuntu lunar-security InRelease [109 kB]
Get:2 http://archive.ubuntu.com/ubuntu lunar InRelease [267 kB]
Get:3 http://security.ubuntu.com/ubuntu lunar-security/main amd64 Packages [533 kB]
Get:4 http://archive.ubuntu.com/ubuntu lunar-updates InRelease [109 kB]
Get:5 http://archive.ubuntu.com/ubuntu lunar-backports InRelease [99.9 kB]
Get:6 http://archive.ubuntu.com/ubuntu lunar/universe amd64 Packages [18.7 MB]
Get:7 http://security.ubuntu.com/ubuntu lunar-security/restricted amd64 Packages [565 kB]
Get:8 http://security.ubuntu.com/ubuntu lunar-security/multiverse amd64 Packages [7154 B]
Get:9 http://security.ubuntu.com/ubuntu lunar-security/universe amd64 Packages [933 kB]
Get:10 http://archive.ubuntu.com/ubuntu lunar/main amd64 Packages [1797 kB]
Get:11 http://archive.ubuntu.com/ubuntu lunar/restricted amd64 Packages [181 kB]
Get:12 http://archive.ubuntu.com/ubuntu lunar/multiverse amd64 Packages [289 kB]
Get:13 http://archive.ubuntu.com/ubuntu lunar-updates/main amd64 Packages [674 kB]
Get:14 http://archive.ubuntu.com/ubuntu lunar-updates/restricted amd64 Packages [570 kB]
Get:15 http://archive.ubuntu.com/ubuntu lunar-updates/multiverse amd64 Packages [7154 B]
Get:16 http://archive.ubuntu.com/ubuntu lunar-updates/universe amd64 Packages [1045 kB]
Get:17 http://archive.ubuntu.com/ubuntu lunar-backports/universe amd64 Packages [4208 B]
Fetched 25.8 MB in 18s (1411 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
The following NEW packages will be installed:
  tree
0 upgraded, 1 newly installed, 0 to remove and 5 not upgraded.
Need to get 45.1 kB of archives.
After this operation, 111 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu lunar/universe amd64 tree amd64 2.1.0-1 [45.1 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 45.1 kB in 1s (55.8 kB/s)
Selecting previously unselected package tree.
(Reading database ... 4353 files and directories currently installed.)
Preparing to unpack .../tree_2.1.0-1_amd64.deb ...
Unpacking tree (2.1.0-1) ...
Setting up tree (2.1.0-1) ...
Removing intermediate container 10090c78c832
 ---> 6939f94bd06b
Successfully built 6939f94bd06b
Successfully tagged myoperation:02
sumit@sumit:~/kemo$ sudo docker image ls
REPOSITORY               TAG       IMAGE ID       CREATED              SIZE
myoperation              02        6939f94bd06b   About a minute ago   113MB
myterminal               00        c315705c381a   16 hours ago         125MB
myterminal               02        8beba2d29e49   16 hours ago         125MB
my_ubuntu_t_g            latest    8627ebf02da2   7 days ago           198MB
tomcat                   latest    fa51ad4dee22   3 weeks ago          460MB
sumitchaubey065/tomcat   latest    fa51ad4dee22   3 weeks ago          460MB
myterminal               01        e4c58958181a   7 weeks ago          77.8MB
ubuntu                   latest    e4c58958181a   7 weeks ago          77.8MB
ubuntu                   23.04     639282825872   7 weeks ago          70.3MB
myoperation              01        639282825872   7 weeks ago          70.3MB
sumit@sumit:~/kemo$ vi dockerfile
RUN pwd>/tmp/1stpwd.txt
RUN cd /tmp/
RUN pwd>/tmp/2ndpwd.txt
sumit@sumit:~/kemo$ sudo docker image build -t myoperation:3.1 .
Sending build context to Docker daemon  2.048kB
Step 1/5 : FROM ubuntu:23.04
 ---> 639282825872
Step 2/5 : RUN apt-get update && apt-get install -y tree
 ---> Using cache
 ---> 6939f94bd06b
Step 3/5 : RUN touch /tmp/1.txt
 ---> Running in d7653aefe442
Removing intermediate container d7653aefe442
 ---> 09eba0e8b768
Step 4/5 : RUN touch /tmp/2.txt
 ---> Running in bd5c1c313a06
Removing intermediate container bd5c1c313a06
 ---> 7ce01433b487
Step 5/5 : RUN touch /tmp/3.txt
 ---> Running in 35b375c7ba9e
Removing intermediate container 35b375c7ba9e
 ---> 7d229711ab15
Successfully built 7d229711ab15
Successfully tagged myoperation:3.1
```
sumit@sumit:~/kemo$ cat dockerfile
```

FROM ubuntu:23.04  
RUN apt-get update && apt-get install -y tree  
RUN touch /tmp/1.txt   
RUN touch /tmp/2.txt  
RUN touch /tmp/3.txt  
```
sumit@sumit:~/kemo$ sudo docker image ls |wc
```
12      83     867

## Docker Volume:
Docker volumes are a way to persist and manage data in Docker containers. They provide a mechanism for storing and sharing data between a container and the host system or between multiple containers. Here are key aspects of Docker volumes:


### Data Persistence:
Docker volumes allow data to persist beyond the lifecycle of a container. This means that even if a container is stopped or removed, the data stored in the volume remains intact.
### Shared Storage:
Volumes provide a way to share data between the host machine and containers. This is useful for scenarios where you want to store configuration files, databases, or other persistent data that should be accessible to the container and the host.
### Container-to-Container Communication:
Volumes enable communication and data sharing between different containers. Multiple containers can mount the same volume, allowing them to access and modify shared data.
### Syntax for Using Volumes:
Volumes can be specified in the Docker run command or in a Docker Compose file using the -v option. 
Code of Docker Volume my sql data persist in docker container
```
sudo docker volume create my_password_volume
```
Creates a Docker volume named my_password_volume. This volume can be used to persistently store data within Docker containers.
[sudo] password for sumit:

Output:  
my_password_volume


```
sudo docker volume ls
```
This is used to list all Docker volumes present on your system. 
OUTPUT:
DRIVER    VOLUME NAME
local     my_password_volume
```
sudo docker image ls
```
[sudo] password for sumit:  
```
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
my_ubuntu_t_g            latest    8627ebf02da2   11 days ago    198MB
tomcat                   latest    fa51ad4dee22   4 weeks ago    460MB
sumitchaubey065/tomcat   latest    fa51ad4dee22   4 weeks ago    460MB
mysql                    latest    a3b6608898d6   4 weeks ago    596MB
```
```
sudo docker image inspect mysql
```
This is used to inspect the details of a Docker image named "mysql."   
[sudo] password for sumit:
```
Output:
[
    {
        "Id": "sha256:a3b6608898d600759effd58768b7213bb44a6d58ab3a53495dd88e6b2042a8a4",
        "RepoTags": [
            "mysql:latest"
        ],
        "RepoDigests": [
            "mysql@sha256:1773f3c7aa9522f0014d0ad2bbdaf597ea3b1643c64c8ccc2123c64afd8b82b1"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2023-10-24T16:24:49Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.16",
                "MYSQL_MAJOR=innovation",
                "MYSQL_VERSION=8.2.0-1.el8",
                "MYSQL_SHELL_VERSION=8.0.35-1.el8"
            ],
            "Cmd": [
                "mysqld"
            ],
            "ArgsEscaped": true,
            "Image": "",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 596207098,
        "VirtualSize": 596207098,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/0972c36305cd82677a4fef7ed57c5433691cbc1f14e5f290dee7dfe3d1af797b/diff:/var/lib/docker/overlay2/4daa28f226ddb1c8793cdb0028cd3946198a5f8fa8b24af119ec1b81b9f72211/diff:/var/lib/docker/overlay2/fd1cb157d0de3a33aa98b6d5f7910c180590b7dac5fd80bccac962d02fb70008/diff:/var/lib/docker/overlay2/d6c5c40aa9fbe9beb40b7c1cbba9b4d895aea40f7a8746755b8eecb2d2256e24/diff:/var/lib/docker/overlay2/ae73e6d3f70b0cd7114e7456c284f256c6c6a458d6f930b247416eb2c246a51d/diff:/var/lib/docker/overlay2/46418f49555b2219010d6099e5fdc8b13c9596e0c4000dbf68a1cf837ee72e81/diff:/var/lib/docker/overlay2/164858893c2d3c05a0df8d2a005bc92bfecd609574b1d4fd594216371ef8e965/diff:/var/lib/docker/overlay2/43ff29390ad5d9071f1f6910a9c82a13637301a8a36ac389ac81685b71d3c2b3/diff:/var/lib/docker/overlay2/f0935090c677b6badb7c31db992576f86c8932269076b962a22ba971ce572e97/diff",
                "MergedDir": "/var/lib/docker/overlay2/49e6a2f20d48fbb0f0972784aae46928f53d45d9cb5f6cb48934e828979a0ec6/merged",
                "UpperDir": "/var/lib/docker/overlay2/49e6a2f20d48fbb0f0972784aae46928f53d45d9cb5f6cb48934e828979a0ec6/diff",
                "WorkDir": "/var/lib/docker/overlay2/49e6a2f20d48fbb0f0972784aae46928f53d45d9cb5f6cb48934e828979a0ec6/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:d6fe63e8be63d078aeef9739f2c7ea101e6cc1a3f998d179af63a10e7f0f959d",
                "sha256:69ef53c77128b4110395e0a6088180a9b721ad4e657519af3833b582be1025e3",
                "sha256:69a630646f651d14ff89d41075d81b1277f918cb11a4f7fe90176bc19be360e2",
                "sha256:9458eec006d024a88859da375e58cba647138e67c0f28929a2e55f57bd7cc059",
                "sha256:01cbaaa2e3b944b563eb5bbbb9c4594ea9ec4bf2c61d4004223df75c6396d3e4",
                "sha256:f159770d104df09d0d2f7c6ba8556cafe3a8e9de56f799c9d5d570bbb8abdb53",
                "sha256:235367ebc71a6828430323a7c33e0c462c6c1fd335fa917625de7145b36ff5af",
                "sha256:867dc881a6cfbc6311a5d77214854881768bf1c34c48fab6a0fa0c82b0c08d50",
                "sha256:1af6e691b2c1e8acb910e66b74cc5e6acb7a995213d796b01ad3dd5587cf9633",
                "sha256:eff93312e2490ea70bca3cdb18a0e653da5432ee287a1f4b8e379f378345465f"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```
```
sudo docker container run -d --name mysql1 -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql
```
```
sudo docker volume inspect 
```
```
b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549
```
This is used to inspect the details of a specific Docker volume with the identifier "b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549." 

```
Output
[
    {
        "CreatedAt": "2023-11-28T16:19:06+05:30",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549/_data",
        "Name": "b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549",
        "Options": null,
        "Scope": "local"
    }
]

```
```
ls -lrth |grep docker
```

This is used to list files in the current directory, sort them by modification time in reverse order (oldest first), and then filter the output to show only those lines containing the word "docker."
```
sudo chown sumit:root docker
```
This is used to change the ownership of the "docker" directory to the user "sumit" and the group "root."
```
cd  /var/lib/docker/volumes/b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549/_data
```
This is used to change the current working directory to the location where the contents of a Docker volume with the identifier "b6a37b57636bbc6502bab2d06e088965f997aa46b4f6a13da07fb6aab6a80549" are stored on the host machine.

```
sudo docker container ls
```
```
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS          PORTS                 NAMES
75369073777f   mysql     "docker-entrypoint.s…"   44 hours ago   Up 17 seconds   3306/tcp, 33060/tcp   mysql1
```
```
sudo docker container exec -it 75369073777f bash
```
This is used to execute an interactive bash shell inside a running Docker container with the container ID or name "75369073777f." 
```
bash-4.4# mysql
```
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.2.0 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.53 sec)
```
```
mysql> create database Sumit;
```
```
Output:
Query OK, 1 row affected (0.39 sec)
```
```
mysql> create database test;
```
Output:
Query OK, 1 row affected (0.24 sec)

```
mysql> create database example;
```
Output:
Query OK, 1 row affected (0.10 sec)

```
mysql> exit
```
Bye
```
Sudo docker container run -itd -v my_password_volume:/var/lib/mysql1 /mysql
```

```
sudo docker container exec -it 18530de2e6752c4d3063247e6b797f4e6da2c28d27078e5565207c05d8ac22fa bash

```
[sudo] password for sumit: 
```
bash-4.4# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.2.0 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| Sumit              |
| example            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
7 rows in set (0.27 sec)
```


## Docker networking:
Docker networking refers to the mechanisms and configurations that enable communication and data exchange between Docker containers and between containers and the external network. Docker provides several networking options to facilitate the seamless interaction of containers. 
Default Bridge Network:
When you run a Docker container without specifying a network, it is connected to the default bridge network.
Containers on the same bridge network can communicate with each other using container names or IP addresses.


### Custom Bridge Networks:
Docker allows you to create custom bridge networks using the docker network create command.
Containers within the same custom bridge network can communicate with each other using container names or service discovery.
### Host Network:
Containers can use the host network mode, sharing the network namespace with the Docker host.
This mode provides maximum network performance but may expose ports directly on the host.
### Overlay Network:
Docker Swarm uses overlay networks for multi-host communication in a cluster.
Containers in the overlay network can communicate seamlessly across multiple nodes.
### Service Discovery and DNS:
Docker includes built-in DNS resolution for container names within the same network.
Containers can discover and communicate with each other using container names.
### Network Inspect and Troubleshooting:
The docker network inspect command allows you to view details of a network, including connected containers.
Troubleshooting commands help diagnose and resolve network-related issues.
```
sudo docker network ls
```
```
Output:
NETWORK ID     NAME      DRIVER    SCOPE
d5973006c25c   bridge    bridge    local
de471c2c59b2   host      host      local
8315440bf14e   none      null      local
```
```
sudo docker container run -itd nginx
```
This command is using Docker to run a detached (in the background) instance of the official Nginx (web server) container in interactive mode.
``` 
Output:
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
1f7ce2fa46ab: Pull complete 
9b16c94bb686: Pull complete 
9a59d19f9c5b: Pull complete 
9ea27b074f71: Pull complete 
c6edf33e2524: Pull complete 
84b1ff10387b: Pull complete 
517357831967: Pull complete 
Digest: sha256:10d1f5b58f74683ad34eb29287e07dab1e90f10af243f151bb50aa5dbb4d62ee
Status: Downloaded newer image for nginx:latest
ea5e63f0e807217dd0660df6e5efe378eeeabca830ed9d3e4ccb5b06b8f80663
```
```
sudo docker container ls
```
```
Output:
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
ea5e63f0e807   nginx     "/docker-entrypoint.…"   39 seconds ago   Up 33 seconds   80/tcp    frosty_fermat
```
```
sudo docker network inspect bridge
```
The command sudo docker network inspect bridge is used to retrieve information about the default bridge network in Docker. 

Output:
```
[
    {
        "Name": "bridge",
        "Id": "d5973006c25c89f622fb6c7ea92f07036cc37b8c7b691a32e5cdebee435cc38b",
        "Created": "2023-12-04T10:41:02.282988+05:30",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "ea5e63f0e807217dd0660df6e5efe378eeeabca830ed9d3e4ccb5b06b8f80663": {
                "Name": "frosty_fermat",
                "EndpointID": "c5984fded82ca3ad9100c01a9b7f981a812118f8683665429bca0a79735fb70e",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```
```
sudo docker container run -it ubuntu
```
This command is using Docker to run an interactive (in the foreground) instance of the official Ubuntu container.  

```
Output:
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
5e8117c0bd28: Pull complete 
Digest: sha256:8eab65df33a6de2844c9aefd19efe8ddb87b7df5e9185a4ab73af936225685bb
Status: Downloaded newer image for ubuntu:latest

```
```
apt-get update && apt-get install iputils-ping
```
This is used to update the package list on a Debian-based Linux system and then install the iputils-ping package.

Output:
```
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.0 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:5 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1027 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
Get:9 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1265 kB]  
Get:10 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1494 kB]                                                                                                               
Get:11 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]                                                                                                                           
Get:12 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]                                                                                                                          
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [49.8 kB]                                                                                                                 
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1294 kB]                                                                                                                   
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1535 kB]                                                                                                                       
Get:16 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [1520 kB]                                                                                                                 
Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [78.3 kB]                                                                                                                     
Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [32.6 kB]                                                                                                                 
Fetched 28.6 MB in 56s (509 kB/s)                                                                                                                                                                         
Reading package lists... Done
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcap2-bin libpam-cap
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 76.8 kB of archives.
After this operation, 280 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libcap2-bin amd64 1:2.44-1ubuntu0.22.04.1 [26.0 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy/main amd64 iputils-ping amd64 3:20211215-1 [42.9 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpam-cap amd64 1:2.44-1ubuntu0.22.04.1 [7928 B]
Fetched 76.8 kB in 2s (42.2 kB/s)   
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 4393 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.44-1ubuntu0.22.04.1_amd64.deb ...
Unpacking libcap2-bin (1:2.44-1ubuntu0.22.04.1) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20211215-1_amd64.deb ...
Unpacking iputils-ping (3:20211215-1) ...
Selecting previously unselected package libpam-cap:amd64.
Preparing to unpack .../libpam-cap_1%3a2.44-1ubuntu0.22.04.1_amd64.deb ...
Unpacking libpam-cap:amd64 (1:2.44-1ubuntu0.22.04.1) ...
Setting up libcap2-bin (1:2.44-1ubuntu0.22.04.1) ...
Setting up libpam-cap:amd64 (1:2.44-1ubuntu0.22.04.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.34.0 /usr/local/share/perl/5.34.0 /usr/lib/x86_64-linux-gnu/perl5/5.34 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.34 /usr/share/perl/5.34 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20211215-1) ...
```
```
ping 8.8.8.8 
```

The ping command is commonly used to test network connectivity and measure the round-trip time for packets to travel from the source to the destination and back.
```
Output:
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=59 time=5.79 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=59 time=12.5 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=59 time=5.61 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=59 time=5.64 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=59 time=14.9 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=59 time=5.87 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=59 time=6.46 ms
64 bytes from 8.8.8.8: icmp_seq=8 ttl=59 time=8.09 ms
64 bytes from 8.8.8.8: icmp_seq=9 ttl=59 time=17.5 ms
64 bytes from 8.8.8.8: icmp_seq=10 ttl=59 time=9.73 ms
64 bytes from 8.8.8.8: icmp_seq=11 ttl=59 time=5.64 ms
64 bytes from 8.8.8.8: icmp_seq=12 ttl=59 time=5.52 ms
64 bytes from 8.8.8.8: icmp_seq=13 ttl=59 time=5.81 ms
64 bytes from 8.8.8.8: icmp_seq=14 ttl=59 time=8.10 ms
64 bytes from 8.8.8.8: icmp_seq=15 ttl=59 time=8.21 ms
64 bytes from 8.8.8.8: icmp_seq=16 ttl=59 time=7.03 ms
64 bytes from 8.8.8.8: icmp_seq=17 ttl=59 time=7.61 ms
64 bytes from 8.8.8.8: icmp_seq=18 ttl=59 time=24.3 ms
64 bytes from 8.8.8.8: icmp_seq=19 ttl=59 time=5.74 ms
64 bytes from 8.8.8.8: icmp_seq=20 ttl=59 time=8.66 ms
64 bytes from 8.8.8.8: icmp_seq=21 ttl=59 time=7.01 ms
64 bytes from 8.8.8.8: icmp_seq=22 ttl=59 time=6.27 ms
64 bytes from 8.8.8.8: icmp_seq=23 ttl=59 time=5.61 ms
64 bytes from 8.8.8.8: icmp_seq=24 ttl=59 time=9.64 ms
64 bytes from 8.8.8.8: icmp_seq=35 ttl=59 time=5.43 ms
^C
--- 8.8.8.8 ping statistics ---
35 packets transmitted, 35 received, +1 duplicates, 0% packet loss, time 34071ms
rtt min/avg/max/mdev = 5.433/14.177/67.789/13.373 ms
```
root@8a43d26e9442:/# ^C
```
sudo docker container run -P -itd nginx
```
is used to run a detached (in the background) instance of the Nginx container with dynamically mapped ports.
[sudo] password for sumit: 
Output:

bc7a7da1f8f2ea9d98785c7f288191cfeae195b94933ee00b1809dad3c7cfd56

```
sudo docker container ls
```
```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
bc7a7da1f8f2   nginx     "/docker-entrypoint.…"   28 seconds ago   Up 19 seconds   0.0.0.0:32768->80/tcp, :::32768->80/tcp   fervent_swartz
8a43d26e9442   ubuntu    "/bin/bash"              20 minutes ago   Up 20 minutes                                             happy_goldstine
ea5e63f0e807   nginx     "/docker-entrypoint.…"   26 minutes ago   Up 25 minutes   80/tcp                                    frosty_fermat
```
```
Ifconfig
```

The ifconfig command is used to display information about network interfaces on a Unix-like system. 

Output:
```
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:b6ff:feea:ac22  prefixlen 64  scopeid 0x20<link>
        ether 02:42:b6:ea:ac:22  txqueuelen 0  (Ethernet)
        RX packets 11043  bytes 582572 (582.5 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20111  bytes 30084801 (30.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp1s0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 54:e1:ad:44:d9:e9  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4350  bytes 400078 (400.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4350  bytes 400078 (400.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth44d56c6: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::2c84:12ff:fe8c:a0e6  prefixlen 64  scopeid 0x20<link>
        ether 2e:84:12:8c:a0:e6  txqueuelen 0  (Ethernet)
        RX packets 11043  bytes 737174 (737.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20130  bytes 30087206 (30.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethdd29b5a: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::a4aa:1aff:fe26:dc6d  prefixlen 64  scopeid 0x20<link>
        ether a6:aa:1a:26:dc:6d  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 37  bytes 4458 (4.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.231  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::8af5:d835:96e9:4157  prefixlen 64  scopeid 0x20<link>
        ether a0:af:bd:d2:56:6d  txqueuelen 1000  (Ethernet)
        RX packets 404909  bytes 426281715 (426.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 140137  bytes 26027855 (26.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

# Docker networking (DNS Enable):

Docker containers can communicate with each other using a variety of networking options. When you mention "DNS Enable," it likely refers to setting up DNS resolution within a Docker network. 

**Default Bridge Network**

By default, Docker containers are connected to a bridge network.Containers on the same bridge network can communicate with each other using container names as hostnames.  

**Custom Bridge Networks**   
You can create custom bridge networks using the docker network create command.Containers on the same custom network can also communicate using container names.  

**User-Defined Networks with DNS Resolution**  

Docker supports user-defined bridge networks with built-in DNS resolution.Containers can resolve each other's names using DNS instead of IP addresses  
**Docker Compose**  

If you're using Docker Compose, you can define networks in the docker-compose.yml file with the dns option.


```
sudo docker network ls
```
command is used to list all the Docker networks on your system. When you run this command, it will display a table with information about each network, including the Network ID, Name, Driver, and Scope.

OUTPUT
```
NETWORK ID     NAME      DRIVER    SCOPE
d5973006c25c   bridge    bridge    local
de471c2c59b2   host      host      local
8315440bf14e   none      null      local
```

```
sudo docker network create test
07e4d7ae7c658c235f04920c78d0c2a65865bc42205058e91141ce2979fe985c
```
To create a custom Docker network, you can use the docker network create command. 
create first container
```
sudo docker container run -it --network=test ubuntu bash
```
Output:
```
root@3a11e0a4784a:/# hostname
3a11e0a4784a
```
```
sudo docker container run -it --network=test ubuntu bash
```
The command sudo docker container run -it --network=test ubuntu bash creates a new Docker container based on the ubuntu image and runs an interactive shell (bash) within that container. The --network=test flag specifies that the container should be connected to the Docker network named test.

Output:
```
root@e282953ada39:/# ping 3a11e0a4784a
```
The command ping 3a11e0a4784a is attempting to ping a hostname or IP address 
```
root@e282953ada39:/# apt-get update && apt-get install iputils-ping
```
Output:  
```

Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]                              
Get:2 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]                                        
Get:3 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.0 kB]
Get:4 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1277 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:6 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1512 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1031 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]      
Get:9 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]         
Get:10 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
Get:11 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
Get:12 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]                                                                                                                               
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [49.8 kB]                                                                                                                 
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [1538 kB]                                                                                                                 
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1302 kB]                                                                                                                   
Get:16 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1550 kB]                                                                                                                       
Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [78.3 kB]                                                                                                                     
Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [32.6 kB]                                                                                                                 
Fetched 28.7 MB in 17s (1689 kB/s)                                                                                                                                                                        
Reading package lists... Done  
Reading package lists... Done  
Building dependency tree... Done  
Reading state information... Done  
The following additional packages will be installed:  
  libcap2-bin libpam-cap  
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 76.8 kB of archives.
After this operation, 280 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libcap2-bin amd64 1:2.44-1ubuntu0.22.04.1 [26.0 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy/main amd64 iputils-ping amd64 3:20211215-1 [42.9 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpam-cap amd64 1:2.44-1ubuntu0.22.04.1 [7928 B]
Fetched 76.8 kB in 1s (69.2 kB/s) 
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 4393 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.44-1ubuntu0.22.04.1_amd64.deb ...
Unpacking libcap2-bin (1:2.44-1ubuntu0.22.04.1) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20211215-1_amd64.deb ...
Unpacking iputils-ping (3:20211215-1) ...
Selecting previously unselected package libpam-cap:amd64.
Preparing to unpack .../libpam-cap_1%3a2.44-1ubuntu0.22.04.1_amd64.deb ...
Unpacking libpam-cap:amd64 (1:2.44-1ubuntu0.22.04.1) ...
Setting up libcap2-bin (1:2.44-1ubuntu0.22.04.1) ...
Setting up libpam-cap:amd64 (1:2.44-1ubuntu0.22.04.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.34.0 /usr/local/share/perl/5.34.0 /usr/lib/x86_64-linux-gnu/perl5/5.34 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.34 /usr/share/perl/5.34 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20211215-1) ...
root@e282953ada39:/# ping 3a11e0a4784a
Output:
PING 3a11e0a4784a (172.20.0.2) 56(84) bytes of data.
64 bytes from 3a11e0a4784a.test (172.20.0.2): icmp_seq=1 ttl=64 time=0.121 ms
64 bytes from 3a11e0a4784a.test (172.20.0.2): icmp_seq=2 ttl=64 time=0.188 ms
64 bytes from 3a11e0a4784a.test (172.20.0.2): icmp_seq=3 ttl=64 time=0.197 ms
64 bytes from 3a11e0a4784a.test (172.20.0.2): icmp_seq=4 ttl=64 time=0.237 ms
^C
--- 3a11e0a4784a ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 0.121/0.185/0.237/0.041 ms
```


# Docker Registry:

A Docker registry is a centralized repository that stores Docker images. It serves as a storage and distribution mechanism for Docker images, allowing users to share and deploy containerized applications. Docker images are built from a set of instructions in a Dockerfile, and they contain everything needed to run a particular application, including the application code, runtime, libraries, and system tools.

## Registry Server
The server that stores and manages Docker images. Popular public registry services include Docker Hub, where users can find and share public images, and private registries that organizations use to store their proprietary images.
Private Registry: Organizations often set up their private Docker registries to store and manage custom images internally. This ensures control over image distribution and versioning.
## Docker Repository 
A collection of related Docker images, usually sharing a common name and organized by tags. Repositories can be part of public or private registries.
```
sudo docker image ls
``````
command is used to list the Docker images that are currently available on your system. 
```
Output: 
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    b6548eacb063   5 days ago    77.8MB
registry     latest    909c3ff012b7   5 days ago    25.4MB
nginx        latest    a6bd71f48f68   2 weeks ago   187MB
```
```
sudo docker container run -d -p 5000:5000 --name simple_registry registry
```
 Command is used to run a Docker container named "simple_registry" based on the "registry" image. 

Output:  
```
ca7803817149ef0b09275f0ae86d7f93b24de1a29b286aeb9329e31ac52cd215
```
```
sudo docker image tag ubuntu:latest 127.0.0.1:5000/ubuntu:latest
```
The command you provided is using Docker to tag an image.
```
sudo docker image ls
```
Output:
```
REPOSITORY              TAG       IMAGE ID       CREATED       SIZE
ubuntu                  latest    b6548eacb063   5 days ago    77.8MB
127.0.0.1:5000/ubuntu   latest    b6548eacb063   5 days ago    77.8MB
registry                latest    909c3ff012b7   5 days ago    25.4MB
nginx                   latest    a6bd71f48f68   2 weeks ago   187MB
```
```
sudo docker image push 127.0.0.1:5000/ubuntu
``````
Command is used to push a Docker image to a specified repository.
Output:
```
Using default tag: latest
The push refers to repository [127.0.0.1:5000/ubuntu]
8ceb9643fb36: Pushed 
latest: digest: sha256:e9bdf73d0872bd8056401aff5a1acdeec598ad3cd16acfbdedd62f36e5f5e0cd size: 529
```
