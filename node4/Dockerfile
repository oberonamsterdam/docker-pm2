FROM keymetrics/pm2:4-alpine
MAINTAINER chin@oberon.nl

ENV NPM_CONFIG_LOGLEVEL warn

RUN apk add --update --no-cache git ca-certificates\
	&& update-ca-certificates \
	&& apk --update add --no-cache openssl openssh bash \
    && rm -rf /tmp/* /var/cache/apk/*

RUN npm -g install pm2

RUN mkdir -p /app
VOLUME /app
WORKDIR /app