FROM php:8.0-cli-alpine3.14

RUN set -ex

# packages that need to be kept installed
RUN apk add --no-cache --update \
    imap-dev \
    libzip-dev \
    libmemcached-dev \
    icu-dev \
    openldap-dev \
    sudo \
    bash \
    supervisor \
    ghostscript \
    gnupg \
    mysql-client \
    git \
    curl \
    wget \
    zip \
    unzip \
    nodejs \
    npm \
    imagemagick \
    libgomp \
    imagemagick-dev \
    libtool

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        freetype-dev \
        libevent-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libxml2-dev \
        curl-dev \
        oniguruma-dev; \
    \
    docker-php-ext-configure gd --with-freetype --with-jpeg; \
    docker-php-ext-configure ldap; \
    docker-php-ext-install -j "$(nproc)" \
        bcmath \
        exif \
        gd \
        intl \
        ldap \
        pcntl \
        pdo_mysql \
        xml \
        soap \
        mbstring \
        imap \
        curl \
        sockets \
        zip; \
    \
    pecl install APCu; \
    pecl install memcached; \
    pecl install redis; \
    pecl install swoole; \
    pecl install imagick; \
    \
    docker-php-ext-enable \
        apcu \
        memcached \
        imagick \
        redis; \
    \
    rm -rf /tmp/*; \
    apk del .build-deps

# install composer
ENV COMPOSER_MEMORY_LIMIT=-1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN npm i -g npm
RUN npm i -g yarn
RUN npm i -g pnpm

RUN addgroup -g 1000 -S noroot && \
    adduser -u 1000 -S noroot -G noroot --home /home/noroot --shell /bin/bash

RUN mkdir -p /home/noroot/.composer/cache
RUN chown -R noroot:noroot /home/noroot

RUN mkdir /app
RUN chown -R noroot:noroot /app;

RUN npm i -g release-it release-it-lerna-changelog

WORKDIR /app
