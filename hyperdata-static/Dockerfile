FROM nginx

RUN apt update && \
	apt upgrade -y && \
	apt install -y wget unzip

WORKDIR /usr/share/nginx/html

#
# get foowiki static files
#
RUN wget -O master.zip https://github.com/danja/foowiki/archive/master.zip

RUN unzip master.zip

RUN mv foowiki-master foowiki

RUN rm master.zip

#
# get schema-editor static files
#
RUN wget -O master.zip https://github.com/zazukoians/schema-editor/archive/gh-pages.zip

RUN unzip master.zip

RUN mv schema-editor-gh-pages/src/public schema-editor-gh-pages

RUN rm master.zip

RUN rm -r schema-editor-gh-pages

###

RUN pwd

RUN ls
