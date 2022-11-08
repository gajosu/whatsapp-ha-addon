# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM
# FROM ghcr.io/hassio-addons/debian-base/amd64:6.1.3

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

WORKDIR /app
COPY ./whatsapp-web-rest-api .

#install node 14 and npm manually
RUN apt-get update && apt-get install -y curl
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
RUN . ~/.nvm/nvm.sh && nvm install 14.17.6 && npm install

#install chromiun
RUN apt-get install -y chromium

# Copy root filesystem
# COPY rootfs /

# RUN apk add ttf-freefont chromium
CMD node index.js

