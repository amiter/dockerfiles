FROM php:7.3-fpm-alpine

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' > /etc/timezone \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk upgrade --update \
    # 安装nginx、依赖库等
    && apk add --no-cache nginx freetype-dev libzip-dev libpng-dev libjpeg-turbo-dev \
    && mkdir -p /run/nginx \
    # 开启nginx gzip
    && sed -i 's/client_max_body_size 1m;/client_max_body_size 20m;/' /etc/nginx/nginx.conf \
    && sed -i 's/#gzip on;/gzip on;/' /etc/nginx/nginx.conf \
    && apk add --no-cache --virtual .build-deps autoconf gcc g++ make dpkg dpkg-dev file libmagic re2c \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-enable opcache \
    && docker-php-ext-install gd zip pdo_mysql bcmath sockets \
    && rm -rf /etc/nginx/conf.d/default.conf \
    && rm -rf /usr/local/etc/php-fpm.d/zz-docker.conf \
    && rm -rf /usr/local/etc/php-fpm.d/www.conf.default \
    # 配置设定
    && sed -i 's#listen = 127.0.0.1:9000#listen = /dev/shm/php-fpm.sock#' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i '/listen.owner = www-data/s/^;//' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i '/listen.group = www-data/s/^;//' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i '/listen.mode = 0660/s/^;//' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's#pm.max_children = 5#pm.max_children = 16#' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's#pm.start_servers = 2#pm.start_servers = 4#' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's#pm.min_spare_servers = 1#pm.min_spare_servers = 2#' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's#pm.max_spare_servers = 3#pm.max_spare_servers = 6#' /usr/local/etc/php-fpm.d/www.conf \
    && echo "post_max_size=20M" >> /usr/local/etc/php/php.ini \
    && echo "upload_max_filesize=20M" >> /usr/local/etc/php/php.ini \
    && echo "memory_limit=1024M" >> /usr/local/etc/php/php.ini \
    && echo "max_input_vars=10000" >> /usr/local/etc/php/php.ini \
    &&  apk del .build-deps \
    && rm -rf /var/cache/apk/* \
    # 安装composer
    && wget -O /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
