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
# RUN wget -O master.zip https://github.com/zazukoians/schema-editor/archive/gh-pages.zip

RUN wget -O master.zip https://github.com/danja/schema-editor/archive/master.zip

RUN unzip master.zip

RUN mv schema-editor-gh-pages/src/public schema-editor-gh-pages

RUN rm master.zip

# RUN rm -r schema-editor-gh-pages

#
# get foolicious static files
#
RUN wget -O master.zip https://github.com/danja/foolicious/archive/master.zip

RUN unzip master.zip

RUN mv foolicious-master foolicious

RUN rm master.zip


###

RUN wget -O master.zip https://github.com/danja/trellis/archive/master.zip

RUN unzip master.zip

RUN mv trellis-master trellis

RUN rm master.zip

RUN pwd

RUN ls
