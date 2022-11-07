# alpine image
ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/base:12.2.6

#install node and npm
RUN apk add --no-cache nodejs npm

#install chromiun
RUN apk add --no-cache chromium

RUN PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

# Copy root filesystem
# COPY rootfs /

# Install Puppeteer under /node_modules so it's available system-wide
WORKDIR /app
COPY ./src .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

