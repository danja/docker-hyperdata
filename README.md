 # docker-hyperdata

Dockerfiles for, ultimately, all my sites and services. Most of this is what I'm now calling **HKMS** *Hyperdata Knowledge Management System*. This stuff isn't exactly production-ready, there are plenty of known bugs, though it all basically works and I'm running it locally for my own daily use.

Most of this is backed by a Fuseki2 SPARQL RDF store.

## Status

*Work in progress!*

**2019-08-26** : Added systemd startup scripts utils/docker-fuseki.service and utils/docker-hyperdata.service.

I found a bug where the URL generation for new pages gained an extra character when working on other hosts, due to me implementing URL parsing in a stupid manner. I *may* have fixed this - haven't got around to properly testing.


2019-04-23 : the local server I was running this on is failing, so I decided to put this on my main laptop. This revealed several issues, so I've made the minimum necessary changes to get it working and edited this doc accordingly.

Mysteriously there were some fragments of a diff in the hyperdata-static Docker file, easily deleted. It looks like the default security has changed in Fuseki2, I was getting HTTP 401 when trying to save pages in FooWiki. So as a workaround for now I've tweaked the Shiro file : **_this shiro.ini allows any user to do a SPARQL update_** so probably best not run on a public server.

**[note to self - BUG: link for FooWiki 'Upload Turtle' is broken, for now have to manually go to http://localhost:3030, choose foowiki dataset, add data, graph: http://hyperdata.it/wiki ]**

So far below there are instructions to get such a store running, with :

* a [FooWiki](https://github.com/danja/foowiki) Wiki instance running as a browser-based application, accessing Fuseki over HTTP.
* [Trellis](https://github.com/danja/trellis) - a browser-based hierarchical todo list, backed by SPARQL store (in development)

## Prerequisites

* Install Docker

* Clone this repo and go into it:

git clone https://github.com/danja/docker-hyperdata.git

cd docker-hyperdata/hyperdata-static

### SPARQL store - Fuseki

*This part sets up a Fuseki2 server with custom config and an associated volume for data persistence.*

* Get the Docker image for Fuseki2 :

sudo docker pull stain/jena-fuseki

* Set up a Busybox container to offer persistent storage volume :

sudo docker run --restart always --name fuseki-data -v /fuseki busybox

* Run a Fuseki container associated with volume:

sudo docker run  --restart always -d --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

(to remove container: sudo docker container rm [container_id])
 
cd hyperdata-static

* Copy the Fuseki config file into the data volume :

sudo docker cp hyperdata-config.ttl fuseki-data:/fuseki/config.ttl

* Copy the Shiro security file into the data volume :

sudo docker cp shiro.ini fuseki-data:/fuseki/shiro.ini

**_NB. this shiro.ini allows any user to do a SPARQL update_**

* Restart into the custom config:

sudo docker stop fuseki

sudo docker start fuseki

navigate to http://localhost:3030

log in as admin:pw123

#### Test Fuseki Server

To check operation of the Fuseki store, first push some data into the store -

Navigate to:

http://localhost:3030/dataset.html

Set these values:

<pre>
Dataset: /foowiki

Endpoint: /foowiki/update
</pre>

In the query box, paste:
<pre>
PREFIX dc: <http://purl.org/dc/elements/1.1/>

INSERT DATA
{ GRAPH <http://hyperdata.it/wiki> {
	<http://example/test>  dc:title  "test" }
}
</pre>
Then click the run arrow. The result should say "Success".

To make sure the data got in the store ok, now enter:
<pre>
Endpoint: /foowiki/query

PREFIX dc: <http://purl.org/dc/elements/1.1/>

SELECT ?value FROM <http://hyperdata.it/wiki>

WHERE {
	<http://example/test>  dc:title ?value
}
</pre>
The result should again be "Success" with the value "test".

### hyperdata-static

*This is based on the nginx image, with various browser-based apps addressing a remote SPARQL store.*

Build nginx-based image, adding files from GitHub :

sudo docker build --no-cache -t hyperdata-static .

Run the Web server on port 80 :

sudo docker run  --restart always --name hyperdata -d -p 80:80 hyperdata-static

#### Accesing FooWiki

This will involve, at minimum, editing the /etc/hosts files on the machines on which you want to access the hyperdata-static material. Within foowiki/config.js the Fuseki server is specified as being at http://fuseki.local:3030

So on the host running FooWiki, /etc/hosts should be edited to add:

127.0.0.1       localhost fuseki.local

Similarly, on other machines on the local network:

127.0.0.1	localhost

192.168.0.150 fuseki.local

Where here 192.168.0.150 is the machine running FooWiki.

Now when navigating to :

http://fuseki.local:3030

you should see the Fuseki admin page. Log in with admin:pw123

Although FooWiki should work as-is, it's recommended to pre-load Fuseki with some bootstrap data (mostly documentation).

Open https://github.com/danja/foowiki and navigate to /etc/bootstrap.ttl - open this **raw** and save it locally somewhere convenient.

In the Fuseki admin interface, on the dataset /foowiki, click 'add data'. The destination graph name is http://hyperdata.it/wiki

Click 'Select Files' and navigate to your copy of bootstrap.ttl

Now opening http://fuseki.local/foowiki should give you a running Wiki with some docs pre-loaded.

#### Startup Scripts (systemd)

To automatically start the corresponding docker services on boot, the files:

* docker-hyperdata/utils/docker-fuseki.service 
* docker-hyperdata/utils/docker-hyperdata.service

should be copied to /etc/systemd/system/ then enabled by executing:

sudo systemctl enable docker-fuseki
sudo systemctl enable docker-hyperdata

#### Removing

sudo docker container stop fuseki
sudo docker container stop fuseki-data
sudo docker container rm fuseki
sudo docker container rm fuseki-data

### Schema Editor

needs a live SPARQL server...

schema-editor-gh-pages/src/public

### newsmonitor

coming soon...

#### notes to self

sudo docker run -dit --restart unless-stopped --name fuseki-data -v /fuseki busybox

sudo docker run -dit --restart unless-stopped --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=pw123 --volumes-from fuseki-data stain/jena-fuseki

// example from docker docs - docker run -dit --restart unless-stopped redis

sudo docker build --no-cache -t hyperdata-static .

https://hub.docker.com/r/stain/jena-fuseki/

https://github.com/stain/jena-docker

attaching a shell:

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
