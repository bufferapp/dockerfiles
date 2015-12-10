FROM tutum/apache-php

MAINTAINER Dan Farrelly <dan@buffer.com>

# Add sources for Mongo drivers
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 ;\
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' \
  | /usr/bin/tee /etc/apt/sources.list.d/mongodb.list

RUN apt-get update
RUN apt-get upgrade
RUN apt-get -yq install \
    php5-dev \
    php5-cli \
    php5-imagick \
    build-essential \
    libmemcached-dev

# Drivers and libraries
RUN /usr/bin/pecl install -f mongo-1.5.8
RUN /usr/bin/pecl install -f memcached-2.0.1
RUN /usr/bin/pecl install -f redis
RUN /usr/bin/pecl install -f xdebug-2.2.5

# Composer
ENV COMPOSER_HOME /app
RUN curl -sS https://getcomposer.org/installer | php ;\
  mv composer.phar /usr/local/bin/composer
# NOTE - Add your composer.json to the /app dir and run composer install, ex:
#   ADD composer.json /app/composer.json
#   RUN composer install

# Configure Apache
COPY php.ini /etc/php5/apache2/php.ini
COPY php.ini /etc/php5/cli/php.ini

RUN /usr/bin/pear config-set php_ini /etc/php5/apache2/php.ini ;\
  /usr/bin/pecl config-set php_ini /etc/php5/apache2/php.ini

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
  /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite
