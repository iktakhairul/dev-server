FROM php:7.4-fpm-alpine
MAINTAINER Iktakhairul <iktakhariul@gmail.com>

ENV PECL_EXTENSIONS="pcov psr redis xdebug"
ENV PHP_EXTENSIONS="bz2 exif gd gettext intl pcntl pdo_mysql zip"

RUN apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS libtool \
    && apk add --no-cache bzip2-dev gettext-dev git icu icu-dev libintl libpng-dev libzip-dev mysql-client \

    && docker-php-source extract \
    && pecl channel-update pecl.php.net \
    && pecl install $PECL_EXTENSIONS \
    && cd /usr/src/php/ext/ \
    && docker-php-ext-enable $PECL_EXTENSIONS \
    && docker-php-ext-configure opcache --enable-opcache \

    && docker-php-ext-install -j "$(nproc)" $PHP_EXTENSIONS \

    && apk del -f .build-deps \
    && cd /usr/local/etc/php/conf.d/ \
    && pecl clear-cache \
    && docker-php-source delete \
    && rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html