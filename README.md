# docker-hyperdata

*Work in progress*

Dockerfiles for, ultimately, all my sites and services.

### Prerequisites

### SPARQL store - Fuseki

-----------------
Basically works, but has wrong config for Fuseki

sudo docker pull stain/jena-fuseki

sudo docker run --name fuseki-data -v /fuseki busybox

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

-----------------
the stain way

as above

cd hyperdata-busybox

sudo docker cp foowiki-config.ttl fuseki-data:/fuseki/config.ttl
// sudo docker cp bootstrap.ttl fuseki-data:/staging/bootstrap.ttl

sudo docker stop fuseki

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki




sudo docker run --name fuseki-data -v /fuseki busyfoo


sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 \
   --volumes-from fuseki-data -v /fuseki stain/jena-fuseki \
  ./tdbloader --desc=/home/foowiki/foowiki-config.ttl ./load.sh foowiki /home/foowiki/bootstrap.ttl

  docker run --volumes-from fuseki-data -v /home/stain/data:/staging stain/jena-fuseki \
    ./tdbloader --desc=/staging/tdb.ttl

-----------------
with tweaked Fuseki Dockerfile, fails silently

cd fooseki

sudo docker build -t fooseki .

sudo docker run --name fuseki-data -v /fuseki busybox

sudo docker run -d --name fooseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data fooseki

java -Xms2048M -Xmx2048M -Xss4m  -jar ./fuseki-server.jar -p 8080:8080 --verbose --update --config ./foowiki-config.ttl


--------------------
sudo docker exec -t -i container_name /bin/bash

https://hub.docker.com/r/stain/jena-fuseki/

sudo docker pull stain/jena-fuseki

* build & run a busybox for data storage, with added config files for foowiki

cd hyperdata-busybox

sudo docker build -t fuseki-data .

sudo docker run --name fuseki-data -v /fuseki fuseki-data

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 \
   --volumes-from fuseki-data -v /fuseki stain/jena-fuseki \
  ./tdbloader --desc=/home/foowiki/foowiki-config.ttl ./load.sh /home/foowiki/bootstrap.ttl

// sudo docker run --name fuseki-data -v /fuseki busybox

// sudo docker run --volumes-from fuseki-data -v /home/stain/data:/staging stain/jena-fuseki \
  ./tdbloader --desc=/staging/tdb.ttl

// sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

navigate to http://localhost:3030

log in as admin:pw123

add new dataset - seki, persistent


https://github.com/stain/jena-docker

## Contents

### hyperdata-static

This is based on the nginx image, with various browser-based apps addressing a remote SPARQL store.

sudo docker build -t foowiki .

sudo docker run --name foowiki -d -p 80:80 foowiki


### newsmonitor



#### note to self

sudo docker exec -t -i container_name /bin/bash

To show only running containers use:

sudo docker ps
To show all containers use:

sudo docker ps -a


removing containers according to pattern

sudo docker ps -a | grep "pattern" | awk '{print $3}' | xargs sudo docker rmi

- total cleanup

list images -

sudo docker images

list containers -

sudo docker ps

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes

stop & remove all containers -

sudo docker stop $(sudo docker ps -a -q)
sudo docker rm $(sudo docker ps -a -q)

remove all images -

sudo docker rmi $(sudo docker images -a -q)

remove all dangling volumes -

sudo docker volume rm $(sudo docker volume ls -f dangling=true -q)
