# alpine image
ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/debian-base:6.1.3

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

#install node 14 and npm
RUN apt-get update && apt-get install nodejs npm -y

#install chromiun
RUN apt-get install -y chromium git

WORKDIR /app

RUN git clone --branch "v2.0.0" --depth=1 \
        "https://github.com/gajosu/whatsapp-web-rest-api.git" /app

# install production dependencies
RUN npm install

RUN npm run build

RUN node -v

CMD node . --supervisorToken=$SUPERVISOR_TOKEN

