FROM --platform=$TARGETOS/$TARGETARCH python:3.11-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN apt update \
    && apt -y install git gcc g++ ca-certificates dnsutils curl wget unzip gnupg2 iproute2 ffmpeg procps tini \
    && useradd -m -d /home/container container \
    && apt-get update -y \
    && apt-get install -y chromium-driver \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /usr/local/bin

# Copie o chromedriver para o local correto
COPY chromedriver /usr/local/bin/chromedriver

# Defina as permissões corretas para o chromedriver
RUN chmod +x /usr/local/bin/chromedriver

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
