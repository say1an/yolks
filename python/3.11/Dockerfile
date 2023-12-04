FROM python:3.11-alpine

LABEL author="Matthew Penner" maintainer="matthew@pterodactyl.io"
LABEL org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL org.opencontainers.image.licenses=MIT

# Install dependencies
RUN apk add --update --no-cache \
    cmake make ca-certificates curl ffmpeg g++ gcc git openssl sqlite tar tzdata \
    && adduser -D -h /home/container container

# Set user and working directory
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Install ChromeDriver
RUN apk add --update --no-cache chromium chromium-chromedriver

# Copy entrypoint script
COPY ./../entrypoint.sh /entrypoint.sh

# Set permissions for entrypoint script
RUN chmod +x /entrypoint.sh

# Define the command to run on container start
CMD ["/bin/ash", "/entrypoint.sh"]
