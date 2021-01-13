FROM php:7.4-cli-buster

ENV COMPOSER_MEMORY_LIMIT=-1

# System
RUN apt-get update && apt-get install -y --no-install-recommends \
        ghostscript \
        nano \
        gnupg \
        sudo \
        git \
        libfreetype6-dev \
        default-mysql-client \
        curl wget \
        zip unzip && \
    # https://docs.spatie.be/laravel-medialibrary/v8/installation-setup/#optimization-tools
    apt-get install -y --no-install-recommends jpegoptim optipng pngquant gifsicle && \
    # ldap installation & configuration
    apt-get install -y --no-install-recommends libldap2-dev && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap && \
    # imagick installation & configuration
    apt-get install -y --no-install-recommends \
        libjpeg62-turbo-dev \
        libpng-dev \
        libmagickwand-dev \
        imagemagick && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install gd && \
    pecl install imagick && \
    docker-php-ext-enable imagick && \
    # zip configuration
    apt-get install -y --no-install-recommends libzip-dev && \
    docker-php-ext-configure zip && \
    docker-php-ext-install zip && \
    # memcached
    apt-get install -y --no-install-recommends libmemcached-dev && \
    pecl install memcached && \
    docker-php-ext-enable memcached && \
    # redis
    pecl install redis && \
    docker-php-ext-enable redis && \
    # apcu
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    # other
    docker-php-ext-configure bcmath && \
    docker-php-ext-install bcmath pdo_mysql exif exif intl mysqli pcntl && \
    # clean things
    apt-get remove -y libzip-dev libfreetype6-dev libpng-dev libmagickwand-dev libjpeg62-turbo-dev && \
    rm -rf /var/lib/apt/lists/*

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash && apt-get install -y nodejs
# install yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN groupadd --gid 1000 noroot && \
    useradd --uid 1000 --gid noroot --shell /bin/bash --create-home noroot

RUN mkdir -p /home/noroot/.composer/cache
RUN chown -R noroot:noroot /home/noroot

RUN mkdir /app
RUN chown noroot:noroot /app

WORKDIR /app
