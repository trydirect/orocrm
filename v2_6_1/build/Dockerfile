FROM ubuntu:16.04

RUN apt-get update -y -qq; apt-get install -y -qq nginx sudo supervisor curl cron git nodejs software-properties-common; \
    LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php -y; apt-get update -y -qq; \
    apt-get install -y php7.1-apcu php7.1-bcmath php7.1-cli php7.1-mbstring php-tidy \
    php7.1-curl php7.1-fpm php7.1-gd php7.1-intl php7.1-mcrypt php7.1-mysql php7.1-soap \
    php7.1-xml php7.1-zip php7.1-imagick php7.1-opcache libfreetype6-dev libmcrypt-dev libpng12-dev; \
    rm -rf /etc/nginx/sites-available/default;rm -rf /etc/nginx/sites-enabled/default;mkdir /run/php; \
    groupadd -r -g 2000  orocrm; useradd -r -u 2000 -g 2000 -m -c "app account" -d /home/orocrm -s /bin/bash orocrm; \
    mkdir -p /opt/letsencrypt

USER orocrm
ENV HOME /home/orocrm
WORKDIR /home/orocrm

USER root
RUN gpasswd -a www-data orocrm
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

USER orocrm
ENV HOME /home/orocrm
WORKDIR /home/orocrm

RUN git clone -b 2.6.13 https://github.com/oroinc/crm-application.git src
RUN cd src && composer install --prefer-dist --no-dev
#COPY ./src/2.6.1.tar.gz /usr/src/app.tar.gz
#RUN chown orocrm:orocrm  /usr/src/app.tar.gz

USER root
RUN chown -R orocrm:orocrm /home/orocrm/.composer
ADD ./configs/php/cli/php.ini /etc/php/7.1/cli/php.ini
ADD ./configs/php/fpm/php.ini /etc/php/7.1/fpm/php.ini
ADD ./configs/php/www.conf /etc/php/7.1/fpm/pool.d/www.conf
ADD configs/supervisord/orocrm.conf /etc/supervisor/conf.d/orocrm.conf

ADD ./scripts/letsencrypt-auto /opt/letsencrypt/letsencrypt-auto
RUN chmod +x /opt/letsencrypt/letsencrypt-auto; /opt/letsencrypt/letsencrypt-auto  --os-packages-only --install-only --non-interactive

RUN apt-get clean; apt-get autoclean; apt-get autoremove -y -qq
