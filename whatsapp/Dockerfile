# alpine image
ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/debian-base/base:6.1.3

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true



#install node 14 and npm
RUN apt-get update && apt-get install -y curl gnupg2 && curl -sL https://deb.nodesource.com/setup_14.x | bash - && apt-get install -y nodejs
#node version
RUN node -v

#install chromiun
RUN apt-get install -y chromium

WORKDIR /app
COPY ./whatsapp-web-rest-api .

RUN npm install

# Copy root filesystem
# COPY rootfs /

# RUN apk add ttf-freefont chromium
CMD node index.js

