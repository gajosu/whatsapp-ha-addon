# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

#add community repository
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories
# ADD so:libcrypto REPOSITORY
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories

#install node and npm
RUN apk add --no-cache nodejs npm


#install chromiun manually
RUN apk add --no-cache chromium

# Copy root filesystem
# COPY rootfs /

WORKDIR /app
COPY ./whatsapp-web-rest-api .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

