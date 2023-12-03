FROM --platform=$TARGETOS/$TARGETARCH python:3.11-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Instale as dependências
RUN apt-get update -y \
    && apt-get install -y git gcc g++ ca-certificates dnsutils curl wget unzip gnupg2 iproute2 ffmpeg procps tini \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Adicione um usuário não privilegiado
RUN useradd -m -d /home/container container

# Crie o diretório para o chromedriver
RUN mkdir -p /usr/local/bin

# Copie o chromedriver para o local correto
COPY chromedriver /usr/local/bin/chromedriver

# Defina as permissões corretas para o chromedriver
RUN chmod +x /usr/local/bin/chromedriver

# Mude para o usuário container
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Configurações adicionais (se necessário)
# ...

# Copie os scripts ou arquivos necessários
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Configuração final
STOPSIGNAL SIGINT
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
