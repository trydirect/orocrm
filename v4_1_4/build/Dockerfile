ARG COMPOSER_VERSION=latest
ARG PHP_VERSION=7.4

FROM composer:${COMPOSER_VERSION} AS composer
FROM php:${PHP_VERSION}-fpm AS base

LABEL maintainer="spilchin.serghei@gmail.com"

COPY --from=composer /usr/bin/composer /usr/bin/composer

FROM base AS dev

ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions apcu bcmath mbstring tidy curl gd intl pcntl mcrypt pdo pdo_mysql mysqli soap xml zip imagick opcache sockets exif imap

# Install dependencies
RUN apt-get update \
        && apt-get install -y --no-install-recommends \
            build-essential \
            mariadb-client\
            default-mysql-client \
            libpng-dev \
            libjpeg62-turbo-dev \
            libfreetype6-dev \
            locales \
            curl \
            unzip \
            supervisor \
            git \
            wget \
            gcc \
            g++ \
            make \
            python-setuptools \
            python-pip \
            librabbitmq-dev \
            software-properties-common \
        && curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh && bash nodesource_setup.sh \
        && apt-get update \
        && apt-get install -y --no-install-recommends nodejs \
        && rm -rf \
            ./nodesource_setup.sh

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8


WORKDIR /var/www

ENV APP_DIR=backend
ARG OROCRM_VERSION=4.1.4

RUN git clone -b ${OROCRM_VERSION} "https://github.com/oroinc/crm-application" ${APP_DIR}
RUN cd ${APP_DIR} && composer install --prefer-dist --no-dev

ADD ./configs/php/php.ini /usr/local/etc/php/php.ini
ADD ./configs/php/www.conf /usr/local/etc/php-fpm.d/www.conf
ADD ./configs/supervisord/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN apt-get clean; apt-get autoclean; apt-get autoremove -y -qq