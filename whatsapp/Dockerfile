# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/debian-base:6.1.3

#install node and npm
RUN apt-get update && apt-get install -y \
    curl \
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs \
    && apt-get install -y build-essential

#install chromiun
RUN apt-get install -y chromium

RUN PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

# Copy root filesystem
# COPY rootfs /

# Install Puppeteer under /node_modules so it's available system-wide
WORKDIR /app
COPY ./src .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

