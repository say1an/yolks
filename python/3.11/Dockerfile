FROM --platform=$TARGETOS/$TARGETARCH python:3.11-alpine

LABEL author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL org.opencontainers.image.licenses=MIT

# Instalação de dependências
RUN apk add --update --no-cache \
    cmake make ca-certificates curl ffmpeg g++ gcc git openssl sqlite tar tzdata \
    && adduser -D -h /home/container container

# Configuração do usuário
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Copia o chromedriver para o local correto
COPY chromedriver /usr/local/bin/chromedriver

# Define as permissões corretas para o chromedriver
RUN chmod +x /usr/local/bin/chromedriver

# Instalação do Chrome
RUN apk add --update --no-cache chromium chromium-chromedriver

# Instalação do Selenium e dependências do Python
RUN apk add --update --no-cache python3 py3-pip \
    && pip3 install selenium

# Copia o entrypoint.sh
COPY ./../entrypoint.sh /entrypoint.sh
CMD ["/bin/ash", "/entrypoint.sh"]
