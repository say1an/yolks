FROM --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL org.opencontainers.image.licenses=MIT

# Instalação de dependências
RUN apk add --update --no-cache \
    ca-certificates curl chromium chromium-chromedriver \
    ffmpeg tzdata

# Configuração do usuário
RUN adduser -D -h /home/container container
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Instalação do Selenium e dependências do Python
RUN apk add --update --no-cache python3 \
    && python3 -m ensurepip \
    && pip3 install --upgrade pip \
    && pip3 install selenium

# Copia o entrypoint.sh
COPY ./../entrypoint.sh /entrypoint.sh
CMD ["/bin/ash", "/entrypoint.sh"]
