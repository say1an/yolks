FROM        --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache cmake make ca-certificates curl ffmpeg g++ gcc git openssl sqlite tar tzdata \
	        && adduser -D -h /home/container container

RUN         apk update && \
		    apk add --no-cache chromium chromium-chromedriver && \
		    rm -rf /var/cache/apk/*

RUN         CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) && \
		    wget -q "https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" && \
		    unzip "chromedriver_linux64.zip" -d /usr/local/bin && \
		    rm "chromedriver_linux64.zip"

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
