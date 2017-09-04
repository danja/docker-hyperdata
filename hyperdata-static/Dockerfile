FROM nginx

RUN apt update && \
	apt upgrade -y && \
	apt install -y wget unzip

WORKDIR /usr/share/nginx/html

RUN wget -O master.zip https://github.com/danja/foowiki/archive/master.zip

RUN unzip master.zip

RUN mv foowiki-master foowiki

RUN rm master.zip

RUN pwd

RUN ls
