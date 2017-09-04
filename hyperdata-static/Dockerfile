FROM nginx

RUN apt update && \
	apt upgrade -y && \
	apt install -y wget unzip

RUN mkdir ./foo-tmp

RUN cd ./foo-tmp

RUN wget -O master.zip https://github.com/danja/foowiki/archive/master.zip

RUN unzip master.zip

RUN mv "foowiki-master" "/usr/share/nginx/html/foowiki"

RUN rm -r ./foo-tmp
