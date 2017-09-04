# docker-hyperdata

*Work in progress*

Dockerfiles for, ultimately, all my sites and services.

### Prerequisites

### SPARQL store - Fuseki

https://hub.docker.com/r/stain/jena-fuseki/

sudo docker pull stain/jena-fuseki

sudo docker run --name fuseki-data -v /fuseki busybox

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

navigate to http://localhost:3030

log in as admin:pw123

add new dataset - seki, persistent


https://github.com/stain/jena-docker

## Contents

### hyperdata-static

This is based on the nginx image, with various browser-based apps addressing a remote SPARQL store.

sudo docker build -t foowiki .

sudo docker run --name foowiki -d -p 80:80 nginx
sudo docker run --name some-nginx -d some-content-nginx
docker run --name some-nginx -d -p 8080:80 some-content-nginx

### newsmonitor



#### note to self


To show only running containers use:

sudo docker ps
To show all containers use:

sudo docker ps -a

To remove all containers that are NOT running

sudo docker rm `docker ps -aq -f status=exited`


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
