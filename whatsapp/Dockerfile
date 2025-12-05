# alpine image
# ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/debian-base:7.8.3

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

#install node 14 and npm
RUN apt-get update && apt-get install -y curl gnupg2 && curl -sL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs git

#install chromiun
RUN apt-get install -y chromium ffmpeg git

# Copy root filesystem
COPY rootfs /

WORKDIR /app

RUN git clone --branch "v2.4.4" --depth=1 \
        "https://github.com/gajosu/ha-whatsapp-web-rest-api.git" /app

# install production dependencies
RUN npm install

RUN npm run build
