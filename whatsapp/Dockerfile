# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM

#install node and npm
RUN apk add --no-cache nodejs npm

#install chromiun
RUN apk add --no-cache chromium

RUN PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

# Copy root filesystem
# COPY rootfs /

WORKDIR /app
COPY ./whatsapp-web-rest-api .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

