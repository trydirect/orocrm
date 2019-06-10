FROM ubuntu:18.04

LABEL maintainer="info@optimum-web.com"
ENV APP_VERSION 2.6.13
ENV HOME /home/orocrm
RUN apt-get update -y -qq;DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y -qq nginx \
    sudo supervisor curl cron git nodejs software-properties-common; \
    LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php -y; apt-get update -y -qq; \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y php7.1-apcu \
    php7.1-bcmath php7.1-cli php7.1-mbstring php-tidy unzip\
    php7.1-curl php7.1-cli php7.1-fpm php7.1-gd php7.1-intl php7.1-mcrypt php7.1-mysql php7.1-soap \
    php7.1-xml php7.1-zip php7.1-imagick php7.1-opcache libfreetype6-dev libmcrypt-dev libpng-dev \
    && apt-get clean; apt-get autoclean; apt-get autoremove -y -qq; \
    rm -rf /etc/nginx/sites-available/default;rm -rf /etc/nginx/sites-enabled/default;mkdir /run/php; \
    groupadd -r -g 2000 orocrm; useradd -r -u 2000 -g 2000 -m -c "app account" -d /home/orocrm -s /bin/bash orocrm; \
    mkdir -p /opt/letsencrypt
ADD ./scripts/letsencrypt-auto /opt/letsencrypt/letsencrypt-auto
RUN chmod +x /opt/letsencrypt/letsencrypt-auto; /opt/letsencrypt/letsencrypt-auto  --os-packages-only --install-only --non-interactive
WORKDIR /home/orocrm
RUN chown -R orocrm:orocrm /home/orocrm
ADD ./configs/php/cli/php.ini /etc/php/7.1/cli/php.ini
ADD ./configs/php/fpm/php.ini /etc/php/7.1/fpm/php.ini
ADD ./configs/php/www.conf /etc/php/7.1/fpm/pool.d/www.conf
ADD configs/supervisord/orocrm.conf /etc/supervisor/conf.d/orocrm.conf
RUN curl -sS https://getcomposer.org/installer | php  && mv composer.phar /usr/bin/composer
USER orocrm
RUN git clone -b ${APP_VERSION} https://github.com/oroinc/crm-application.git src
RUN cd src && composer install --prefer-dist --no-dev
USER root
