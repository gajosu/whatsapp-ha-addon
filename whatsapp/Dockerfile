# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

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

