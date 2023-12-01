# Use a imagem Python 3.11 slim
FROM python:3.11-slim

# Informações do autor e mantenedor
LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Atualiza os pacotes e instala dependências necessárias
RUN apt-get update \
    && apt-get -y install git gcc g++ ca-certificates dnsutils curl wget unzip iproute2 ffmpeg procps tini \
    && useradd -m -d /home/container container

# Define o usuário e diretório de trabalho
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Baixa e instala o ChromeDriver
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo "deb [signed-by=/usr/share/keyrings/google-chrome-archive-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update -y \
    && apt-get install -y google-chrome-stable \
    && CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) \
    && wget -q "https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" \
    && unzip "chromedriver_linux64.zip" -d /usr/local/bin \
    && rm "chromedriver_linux64.zip"

# Define uma variável de ambiente para o caminho do ChromeDriver
ENV CHROME_DRIVER_PATH /usr/local/bin/chromedriver

# Copia o script de entrada
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Configura o ponto de entrada
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]

# Comando padrão
CMD ["/entrypoint.sh"]
