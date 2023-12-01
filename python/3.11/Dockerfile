
FROM python:3.11-slim


LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"


RUN apt-get update -y \
    && apt-get install -y git gcc g++ ca-certificates dnsutils curl wget unzip gnupg2 iproute2 ffmpeg procps tini \
    && useradd -m -d /home/container container 


RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/google-chrome-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/google-chrome-archive-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update -y \
    && apt-get install -y google-chrome-stable \
    && apt-key --keyring /usr/share/keyrings/google-chrome-archive-keyring.gpg adv --refresh-keys

RUN CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) \
    && wget -q "https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" \
    && unzip "chromedriver_linux64.zip" -d /usr/local/bin \
    && rm "chromedriver_linux64.zip"


ENV CHROME_DRIVER_PATH /usr/local/bin/chromedriver


USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container


STOPSIGNAL SIGINT


COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh


ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
