FROM        --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache cmake make ca-certificates curl ffmpeg g++ gcc git openssl sqlite tar tzdata \
	        && adduser -D -h /home/container container
		 
RUN         apt-get install -y wget xvfb unzip

RUN         wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN         echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list


RUN         apt-get update -y
RUN         apt-get install -y google-chrome-stable


ENV         CHROMEDRIVER_VERSION 2.19
ENV         CHROMEDRIVER_DIR /chromedriver
RUN         mkdir $CHROMEDRIVER_DIR


RUN         wget -q --continue -P $CHROMEDRIVER_DIR "http://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip"
RUN         unzip $CHROMEDRIVER_DIR/chromedriver* -d $CHROMEDRIVER_DIR


ENV         PATH $CHROMEDRIVER_DIR:$PATH

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
