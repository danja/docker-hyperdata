FROM java:8-jre-alpine

RUN apk add --update pwgen bash wget unzip ca-certificates && rm -rf /var/cache/apk/*

RUN wget -O master.zip https://github.com/danja/newsmonitor/archive/master.zip

RUN unzip master.zip
