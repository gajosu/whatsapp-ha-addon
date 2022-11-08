# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM
# FROM ghcr.io/hassio-addons/debian-base/amd64:6.1.3

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

WORKDIR /app
COPY ./whatsapp-web-rest-api .

#install node 14 and npm manually

RUN apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs && \
    apt-get install -y build-essential && \
    apt-get install -y chromium && \
    npm install

# Copy root filesystem
# COPY rootfs /

# RUN apk add ttf-freefont chromium
CMD node index.js

