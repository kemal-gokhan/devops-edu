# Docker Basics

**Docker Engine** = Docker CLI + REST API + Docker Deamon (Docker CLI could be seperately in hardware.)
Containerizations with namespaces. In container, there is a child system in the Linux.
Thanks to layered architecture, you dont need to high size storage for images.
<mark>var/lib/docker/-> /aufs /container /image </mark>
1.volume mounting
2.bind mounting (bind to directory)

**Storage drivers:** AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2

### In sequence useful commands

`docker info | grep Storage Driver`

`docker history <container name>`

`docker run --name=nginx1 nginx`

`docker run -d nginx `(detach-background)

`docker stop nginx`

`dcker rm <name>`

`docker ps -a`

`docker images`

`docker rmi nginx` (remove images)

`docker pull nginx`

`docker run ubuntu sleep 10`

`docker exec <name>` cat /etc/host (execute)

`docker run -i nginx`

`docker run -it nginx`

`docker run -p host:container nginx`

`docker run -p 80:5000 nginx`

docker volume create data_volume ( /var/lib/docker/volumes/data_volume)
docker run -v /opt/datadir:var/lib/mysql mysql = docker run \ --mount type=bind, source=opt/datadir:var/lib/mysql mysql
docker run -v host:container nginx

`docker system df -v` (show docker how much data stored)

`docker run --cpus=5 --memory=100m ubuntu`

`docker inspect <name>`

`docker logs <name>`

`docker run -e APP_COLOR=blue <name>` (environmental value)

### Dockerfile

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

`docker build Dockerfile -t kemal/myappnewbuild`

`docker push localhost:5000/my-image`

`docker pull localhost:5000/my-image`

**networks**:

1. Bridge default
2. None --network none
3. Host --network host
   `docker run ubuntu --network=none`

   `docker network ls`

### Docker compose:

services:
 web:
 image: "nginx"
docker-compose up

image: registry-useracc-imagerepo

docker login

### Swarm:

`docker swarm init`

`docker swarm join --token <token>`

`docker services create --replicas=100 nginx`

**Docker swarm**:
Swarm Manager (orchestrator)
Worker 1 - Worker 2 - Worker 3 **example**:
 docker service create --replicas=3 --network frontend -p 8080:80 nginx

### k8s

`kubectl rolling-update nginx --rollback`

`kubectl run nginx`

`kubectl run nginx --image=nginx --replicas=10`

**Example docker commands**:
`docker run -p 38282:8080 --name blue-app -e APP_COLOR=blue -d kodekloud/simple-webapp`

`docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d mysql`

`docker run -d ubuntu sleep 1000`

`docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql`

`docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d -v /opt/data:/var/lib/mysql mysql`

`docker network ls`

`docker inspect network alpine-1 | grep -i -n netw`

`docker network inspect bridge`

`docker network create --driver bridge --subnet 182.18.0.1/24 --gateway 182.18.0.1 wp-mysql-network`

`docker run -d --name mysql-db --network wp-mysql-network -e MYSQL_ROOT_PASSWORD=db_pass123 mysql:5.6`

`docker run --name webapp -d -e DB_Host=mysql-db -e DB_Password=db_pass123 --network wp-mysql-network --link mysql:webapp -p 38080:8080 kodekloud/simple-webapp-mysql `