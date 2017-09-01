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

sudo docker run foowiki -d -p 80:80

### newsmonitor



#### note to self - total cleanup

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
