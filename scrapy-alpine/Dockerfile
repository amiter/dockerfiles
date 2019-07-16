FROM python:3.7-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk upgrade --update \
    && apk add --no-cache --virtual .build-deps linux-headers gcc libc-dev \
    && apk add --no-cache openssl-dev libxml2-dev libxslt-dev libffi-dev \
    && pip install --upgrade pip \
    && pip install scrapy \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/*
