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

#install chromium-browser
RUN apt-get update && apt-get install -y chromium

RUN apt-get install -y gconf-service libgbm-dev libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget


# Copy root filesystem
# COPY rootfs /

# Install Puppeteer under /node_modules so it's available system-wide
WORKDIR /app
COPY ./src .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

