FROM keymetrics/pm2:6-alpine
MAINTAINER chin@oberon.nl

ENV NPM_CONFIG_LOGLEVEL warn

RUN apk add --update --no-cache git ca-certificates\
	&& update-ca-certificates \
	&& apk --update add --no-cache openssl openssh bash \
    && rm -rf /tmp/* /var/cache/apk/*

RUN mkdir -p /app
VOLUME /app
WORKDIR /app