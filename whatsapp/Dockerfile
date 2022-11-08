# alpine image
ARG BUILD_FROM

FROM $BUILD_FROM

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

RUN apt-get update && apt-get install -y \
    curl \
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs \
    && apt-get install -y build-essential \
    && apt-get install npm



#install chromiun
RUN apt-get install -y chromium

# Copy root filesystem
# COPY rootfs /

WORKDIR /app
COPY ./whatsapp-web-rest-api .
RUN npm install
# RUN apk add ttf-freefont chromium
CMD node index.js

