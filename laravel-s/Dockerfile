FROM php:7.3-cli-alpine3.10

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' > /etc/timezone \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
	&& apk upgrade --update \
	&& apk add --no-cache libstdc++ \
    && apk add --no-cache --virtual .build-deps linux-headers autoconf gcc g++ php7-pear php7-dev musl-dev make libxml2-dev \
    && pecl install swoole \
    && docker-php-ext-enable swoole \
    && docker-php-ext-install pdo_mysql mbstring bcmath pcntl \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/*