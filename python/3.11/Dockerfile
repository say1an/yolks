FROM        --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache cmake make ca-certificates curl wget xvfb unzip ffmpeg g++ gcc git openssl sqlite tar tzdata \
	        && adduser -D -h /home/container container
		 
RUN         docker run -d -p 4444:4444 --shm-size="2g" selenium/standalone-chrome:4.15.0-20231129

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
