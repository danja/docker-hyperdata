# docker-hyperdata

*Work in progress*

Dockerfiles for, ultimately, all my sites and services.

### SPARQL store - Fuseki

*This part sets up a Fuseki2 server with custom config and an associated volume for data persistence.*

* Get the Docker image for Fuseki2 :

sudo docker pull stain/jena-fuseki

* Set up a Busybox container to offer persistent storage volume :

sudo docker run --name fuseki-data -v /fuseki busybox

* Run a Fuseki container associated with volume:

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

cd hyperdata-static

* Copy the Fuseki config file into the data volume :

sudo docker cp hyperdata-config.ttl fuseki-data:/fuseki/config.ttl

sudo docker stop fuseki

* Restart using the custom config:

sudo docker run -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki


navigate to http://localhost:3030

log in as admin:pw123

### hyperdata-static

*This is based on the nginx image, with various browser-based apps addressing a remote SPARQL store.*

cd hyperdata-static

Build nginx-based image, adding files from GitHub :

sudo docker build -t hyperdata-static .

Run the Web server on port 80 :

sudo docker run --name hyperdata -d -p 80:80 hyperdata-static

### newsmonitor

coming soon...

#### notes to self

https://hub.docker.com/r/stain/jena-fuseki/

https://github.com/stain/jena-docker

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
