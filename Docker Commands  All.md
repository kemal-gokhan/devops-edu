Docker daemon < docker Rest API < docker cli

docker login

docker info

docker run containerimagename = docker container run containerimagename (docker object ---- )

##### docker ----- management command --- command

docker container run --name first container containerimagename 

docker hub is auto repos for docker engine.

docker container ls -a

docker container logs containerid

docker contaner run -d -p 80:80 containerimagename

docker container rm containerid

docker container rm -f containerid

docker container stop containerid

docker container exec -it containername   (sh = shell )

Union File System

docker container prune (remove all)

docker image pull imagename

docker volume create volumename (Mountpoint: is the place that keeps data)

docker volume ls

docker volume inspect volumename

docker container -it -v volume1:containerlocation alphine 

docker container -it -v volume1:mnt/volume alphine (you can attach one volume to more container)

docker container run --rm -it containerimage (after exit, delete docker)

BindMount use for personal purpose. It can access your personal directory on your docker run pc.

docker container run -d -p 80:80 -v C:\Users\Kemal:var\html nginx

### Driver:

-Bridge Network
-Host Network
-MacVLAN
-None
-OVerlay (DockerSwarm)

docker network ls

docker network inspect Bridge

docker contaiiner run -it --name poc1 --net host imagename
--Control pq is exit method without stop container

-p host_port:container_port

docker network create --driver bridge bridgename

docker attach containername (container to container)

docker network create --driver bridge --subnet 10.0.0.0/24 --ip-range 10.0.0.0/24 --gateway 10.0.0.1 bridgename1

docker network connect bridgename containername

docker network disconnect bridgename containername

docker logs containername

docker logs -t containername

docker logs --until timestamp containername

docker top containername

docker stats
docker stats containernam

docker container run -d --cpus "1" --memory 100m --memory-swap 200m mcontainername

printenv

--env or -e or --env-file path

### image registry:

registry-url / repository / tag

docker.io/library/ubuntu:şatest
gcr.io/google-containers/busybox:latest

### Commands

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

docker image build -t reposityoryname -f Dockerfile .

docker image history containername

In sequence is critical in order to build fast images.

HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1

the fewer instructions, the less image layers.

### ADD / COPY

COPY -> SOURCE DEST

ADD -> SAME AS COPY + URL + EXTRACT TAR

### ENTRYPOINT / CMD

ENTRYPOINT CAN NOT CHANGE ON RUNTIME. (CLI)
IF EXIST TOGETHER *ENTRYPOINT* IS COMMAND *CMD* IS PARAMETER.SHOULD BE EXEC FORMAT.

EXEC FORM [""] - SHELL FORM -

### 

docker cp containername:location destination

COPY --from=nginx latest source destination

FROM mcr.microsoft.com AS shortcut
...
...
...

COPY --from=shortcutsource destination

ARG 
