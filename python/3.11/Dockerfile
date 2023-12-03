FROM        --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache cmake make ca-certificates curl ffmpeg g++ gcc git openssl sqlite tar tzdata \
	        && adduser -D -h /home/container container

RUN         pip install --trusted-host pypi.python.org -r requirements.txt

RUN         apt-get update && apt-get install -y wget unzip && \
            wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
			rm google-chrome-stable_current_amd64.deb && \
            apt-get clean

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
