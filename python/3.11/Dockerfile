FROM        --platform=$TARGETOS/$TARGETARCH python:3.11-slim

LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN         apt update \
            && apt -y install git gcc g++ ca-certificates dnsutils curl wget unzip gnupg2 iproute2 ffmpeg procps tini \
            && useradd -m -d /home/container container

RUN         wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
		    && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \
		    && apt-get update -y \
		    && apt-get install -y google-chrome-stable \
		    && apt-get clean \
		    && rm -rf /var/lib/apt/lists/* \
		    && CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) \
		    && wget -q "https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" \
		    && unzip "chromedriver_linux64.zip" -d /usr/local/bin \
		    && rm "chromedriver_linux64.zip"


RUN         pip install selenium
			
RUN         apt-get update -y \
		    && apt-get install -y wget unzip python3-pip \
		    && CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) \
		    && wget -q "https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" \
		    && unzip "chromedriver_linux64.zip" -d /usr/local/bin \
		    && rm "chromedriver_linux64.zip" \
		    && pip3 install selenium
	  
ENV         CHROME_DRIVER_PATH /usr/local/bin/chromedriver

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
