# Docker Basics

**Docker Engine** = Docker CLI + REST API + Docker Deamon (Docker CLI could be seperately in hardware.)
Containerizations with namespaces. In container, there is a child system in the Linux.
Thanks to layered architecture, you dont need to high size storage for images.
<mark>var/lib/docker/-> /aufs /container /image </mark>
1.volume mounting
2.bind mounting (bind to directory)


**Storage drivers:** AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2

Docker daemon < docker Rest API < docker cli

### First

`docker login`

`docker info`

`docker info | grep Storage Driver`

`docker history <container name>`

`docker run containerimagename = docker container run containerimagename (docker object ---- )`

##### docker ----- management command --- command

`docker container run --name first container containerimagename` 

`docker hub is auto repos for docker engine.

`docker container ls -a`

`docker container logs containerid`

`docker contaner run -d -p 80:80 containerimagename`

`docker container rm containerid`

`docker container rm -f containerid`

`docker container stop containerid`

`docker container exec -it containername`   (sh = shell )

##### Union File System - Volume

`docker container prune ` (remove all)

`docker image pull imagename`

`docker volume create volumename` (Mountpoint: is the place that keeps data)

`docker volume ls`

`docker volume inspect volumename`

`docker container -it -v volume1:containerlocation alphine `

`docker container -it -v volume1:mnt/volume alphine` (you can attach one volume to more container)

`docker container run --rm -it containerimage` (after exit, delete docker)

`docker volume create data_volume` ( /var/lib/docker/volumes/data_volume)

```docker run -v /opt/datadir:var/lib/mysql mysql = docker run \ 
--mount type=bind, source=opt/datadir:var/lib/mysql mysql
```

`docker run -v host:container nginx`

`docker system df -v` (show docker how much data stored)

BindMount use for personal purpose. It can access your personal directory on your docker run pc.

`docker container run -d -p 80:80 -v C:\Users\Kemal:var\html nginx`

### Driver:

-Bridge Network
-Host Network
-MacVLAN
-None
-OVerlay (DockerSwarm)

`docker network ls`

`docker network inspect Bridge`

`docker contaiiner run -it --name poc1 --net host imagename`
--Control pq is exit method without stop container

`-p host_port:container_port`

`docker network create --driver bridge bridgename`

`docker attach containername (container to container)`

`docker network create --driver bridge --subnet 10.0.0.0/24 --ip-range 10.0.0.0/24 --gateway 10.0.0.1 bridgename1`

`docker network connect bridgename containername`

`docker network disconnect bridgename containername`

`docker logs containername`

`docker logs -t containername`

`docker logs --until timestamp containername`

`docker top containername`

`docker stats`

`docker stats containernam`

`docker container run -d --cpus "1" --memory 100m --memory-swap 200m mcontainername`

`printenv`

--env or -e or --env-file path

### image registry:

registry-url / repository / tag

docker.io/library/ubuntu:şatest
gcr.io/google-containers/busybox:latest

### Commands
```
FROM: BASE IMAGE

RUN: COMMAND WHILE POWER ON - CREATING IMAGE 

WORKDIR: CD

COPY: COPY TO IMAGE

ADD: ADD FILE

ARG: CREATE ARG

EXPOSE: EXPOSE COMMAND

CMD: AFTER POWER ON, RUN CMD

ENTRYPOINT: .

HEALTHCHECK: HEALTHCHECK

ENV: CREATE ENV-IMPORTANT

LABEL: ADD METADATA

ARG: VARIABLENAME THEN  ${VARIABLENAME} JUST FOR USE FOR CREATING CONTAINER CANNOT USE IN CONTAINER -> ENV
```

```

CMD [ "10" ]
ENTRYPOINT [ "sleep" ]

CMD [ "sleep", "5" ]

IMAGES:
FROM Ubuntu
RUN apt-get update
RUN apt-get install python
RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code (copy source dest)
ENTRYPOINT [ "FLASK_APP=/opt/source-code/app.py flask run" ]
```



`docker image build -t reposityoryname -f Dockerfile .`

`docker image history containername`

In sequence is critical in order to build fast images.

`HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1`

the fewer instructions, the less image layers.

### ADD / COPY

COPY -> SOURCE DEST

ADD -> SAME AS COPY + URL + EXTRACT TAR

### ENTRYPOINT / CMD

ENTRYPOINT CAN NOT CHANGE ON RUNTIME. (CLI)
IF EXIST TOGETHER *ENTRYPOINT* IS COMMAND *CMD* IS PARAMETER.SHOULD BE EXEC FORMAT.

EXEC FORM [""] - SHELL FORM -

### COPY and ARG

`docker cp containername:location destination`

```
COPY --from=nginx latest source destination

FROM mcr.microsoft.com AS shortcut
...
...
...

COPY --from=shortcutsource destination

ARG 
```

`docker run -e APP_COLOR=blue <name>`


### Docker Commit

This is another way to build docker images.

`docker commit containername tag`

`docker commit -c ' CMD ["java", "uygulama"] ' containername tag`

### Save & Load

save container as tar. and load for without internet connection

`docker save containername -o filename.tar`

`docker load -i .filename.tar`

### Docker Registery

`docker pull registry`

https://hub.docker.com/_/registry

`docker run -d -p 5000:5000 --restart always --name registry registry:2`

`docker image tag abc-useruser/latest 127.0.0.1:5000/hello-app:latest` 

`docker image push 127.0.0.1:5000/hello-app:latest `

### Work

docker container ls -a

docker container prune

docker image ls -a

docker image prune (dangling images)

docker image prune -a

docker login

docker image tag 879230ec0d0d  kemalgokhan/gitlab

docker image pull centos

docker image pull ubuntu

docker image tag ubuntu kemalgokhan/exercise:ubuntu

docker image push kemalgokhan/exercise:ubuntu
