---
title: Server 2016 Containers
description: A quick summary of Containers
published: true
date: 2020-02-06T23:18:58.425Z
tags: 
---

# Server 2016 Containers

## Basic Commands



### Powershell

```
### CN1-NUG (Desktop) ###

# enter remote session
Enter-PSSession CN1-NUG

# install docker provider
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force

# install docker
Install-Package -Name docker -ProviderName DockerMsftProvider -Force

# reboot
Restart-Computer -Force

# docker install & root dir
Get-ChildItem -Name 'C:\Program Files\Docker'
Get-ChildItem -Name 'C:\ProgramData\docker'

# docker daemon
Get-Service -Name docker

# docker client
docker version
docker info

# configure docker daemon
$json = '{ "graph": "c:\\docker" }'
$json | Set-Content c:\programdata\docker\config\daemon.json
New-Item -ItemType Directory -Path c:\docker

# restart service
Restart-Service -Name docker


### CN2-NUG (Nano) ###

# enter remote session
Enter-PSSession -ComputerName CN2-NUG

# install docker provider
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force

# install docker
Install-Package -Name docker -ProviderName DockerMsftProvider -Force

# install the hyper-v role
Install-NanoServerPackage -Name Microsoft-NanoServer-Compute-Package

# reboot
Restart-Computer -Force

# docker install & root dir
Get-ChildItem -Name 'C:\Program Files\Docker'
Get-ChildItem -Name 'C:\ProgramData\docker'

# docker daemon
Set-Service -Name docker -StartupType Automatic
Get-Service -Name docker | Start-Service

# docker client
docker version
docker info

# configure docker daemon
$json = '{ "graph": "c:\\docker" }'
$json | Set-Content c:\programdata\docker\config\daemon.json
New-Item -ItemType Directory -Path c:\docker

# restart service
Restart-Service -Name docker
```

## Deploy

### Powershell

```


### CN1-NUG (Desktop) ###

# enter remote session
Enter-PSSession CN1-NUG

# get help on docker client
docker --help

# search images on docker hub
docker search microsoft

# download base os images
docker pull microsoft/windowsservercore
docker pull microsoft/nanoserver

# download tagged version of image
docker pull microsoft/nanoserver:10.0.14393.1480

# view local images
docker images

# create background windows container
docker run microsoft/windowsservercore ping -t localhost

# create interactive windows container (unsupported via remote session)
docker run -it microsoft/windowsservercore powershell

# view running containers
docker ps -a

# create image from container
docker commit <name_or_id> nuglab/iis

# create a tagged image from container
docker commit <name_or_id> nuglab/iis:1.0

# view container state and settings
docker inspect <name_or_id>

# stop container
docker stop <name_or_id>

# delete container
docker rm <name_or_id>

# delete local image
docker rmi -f microsoft/nanoserver:10.0.14393.1480


### CN2-NUG (Nano) ###

# enter remote session
Enter-PSSession -ComputerName CN2-NUG

# download and install powershell for containers (http://cbt.gg/2uZtKFn)
Register-PSRepository -Name DockerPS-Dev -SourceLocation https://ci.appveyor.com/nuget/docker-powershell-dev
Install-Module Docker -Repository DockerPS-Dev -Scope AllUsers -Force
Get-Command -Module Docker
Update-Module -Name Docker

# install base os images
Request-ContainerImage -Repository microsoft/nanoserver

# view local images
Get-ContainerImage

# create windows container
New-Container -Name WindowsContainer -ImageIdOrName microsoft/nanoserver -Command powershell

# create hyper-v container
New-Container -Name HyperVContainer -ImageIdOrName microsoft/nanoserver -Isolation HyperV -Command powershell

# view containers
Get-Container

# windows vs hyper-v containers
docker run microsoft/nanoserver ping -t localhost
docker run --isolation=hyperv microsoft/nanoserver ping -t localhost

# create image from container
ConvertTo-ContainerImage -ContainerIdOrName HyperVContainer -Repository nuglab/nano -Tag 1.0

# view container state and settings
Get-ContainerDetail -ContainerIdOrName WindowsContainer

# stop all containers
Get-Container | Stop-Container

# delete all containers
Get-Container | Remove-Container

# delete local image
Remove-ContainerImage -ImageIdOrName nuglab/nano:1.0 -Force
```

## Management

To leave a container without detaching in windows. CTRL + P + Q.


### Powershell
```


# enter remote session
Enter-PSSession CN1-NUG


## Container Networking ##

# view networks
docker network ls

# view default nat network details
docker network inspect nat

# start a container
docker run -dt nuglab/iis

# start a container with port mapping
docker run -dt -p 8080:80 nuglab/iis

# create a transparent network
docker network create -d transparent -o com.docker.network.windowsshim.interface="Ethernet 4" --subnet=192.168.1.0/24 --gateway=192.168.1.1 tpnet

# start a container on tpnet
docker run -dt --network=tpnet --ip=192.168.1.130 --dns=192.168.1.100 microsoft/windowsservercore


## Container Data Volumes ##

# create a persistent data volume
docker volume create nugdata

# start containers using data volume
docker run --name web1 -dt -v nugdata:c:\data nuglab/iis
docker run --name web2 -dt -v nugdata:c:\data nuglab/iis

# send command into container
docker exec <name_or_id> powershell dir c:\

# view data volumes
docker volume ls

# mount host dir into container
docker run --name web3 -dt -v c:\nuggetlab:c:\mounted nuglab/iis

# send command into container
docker exec <name_or_id> powershell dir c:\


## Container Resource Control ##

# view docker run help
docker run --help

# limit memory & cpu
docker run -dt --memory 512m --cpu-percent 25 nuglab/iis


## Dockerfile - Image Automation ##

# build image from dockerfile
docker tag nuglab/iis nuglab/iis:1.0
docker build -t nuglab/iis .
docker run -dt -p 8080:80 nuglab/iis:latest

## Dockerhub - Image Repo ##

# login to dockerhub
docker login

# push image to nuglab/iis repo
docker push nuglab/iis:latest
```