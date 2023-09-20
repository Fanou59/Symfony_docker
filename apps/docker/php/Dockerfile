# ./docker/php/Dockerfile

FROM composer:2.4.2 as composer

##################################

FROM php:8.2-fpm-alpine3.17
    
RUN apk add --no-cache bash git icu-dev
  

RUN mkdir -p /usr/src/app \
    && apk add --no-cache --virtual=.build-deps \
        autoconf \
        g++ \
        make \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j"$(nproc)" intl pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu intl \
    && apk del .build-deps

WORKDIR /usr/src/app

COPY apps/my-symfony-app/composer.json /usr/src/app/composer.json
COPY apps/my-symfony-app/composer.lock /usr/src/app/composer.lock
    
RUN PATH=$PATH:/usr/src/app/vendor/bin:bin

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN composer install --no-scripts

COPY apps/my-symfony-app /usr/src/app

RUN chown -R 501:20 /usr/src/app
USER 501:20