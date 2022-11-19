# alpine image
ARG BUILD_FROM

# FROM $BUILD_FROM
FROM ghcr.io/hassio-addons/base:12.2.7


ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

#install node and npm
RUN apk add --no-cache nodejs npm

#install chromiun manually
RUN apk add --no-cache chromium


# Copy root filesystem
# COPY rootfs /

WORKDIR /app

RUN git clone --branch "v2.0.0" --depth=1 \
        "https://github.com/gajosu/whatsapp-web-rest-api.git" /app

# install production dependencies
RUN npm install

RUN npm run build

RUN node -v

CMD node . --supervisorToken=$SUPERVISOR_TOKEN
