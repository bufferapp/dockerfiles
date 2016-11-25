FROM php:5.5.35-apache
MAINTAINER Dan Farrelly <dan@buffer.com>

RUN apt-get update
RUN apt-get -yq install \
      build-essential \
      libmemcached-dev \
      libmcrypt-dev \
      php-pear \
      libcurl3 \
      php5-dev \
      php5-curl \
      php5-cli \
      php-calendar \
      php5-imagick \
      php5-gd

RUN docker-php-ext-install mcrypt

# Drivers and libraries
RUN pecl install -f mongo-1.5.8
RUN pecl install -f memcached-2.2.0
RUN pecl install -f redis-2.2.8
RUN docker-php-ext-enable mongo memcached redis

# Configure Apache
COPY php.ini /usr/local/etc/php/

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
  /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

# Log folder for metrics and elastic search logging
RUN mkdir -p /var/log/buffer
RUN chmod 777 /var/log/buffer

EXPOSE 80
EXPOSE 443

# Configure /app folder w/ symlink
RUN mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html

WORKDIR /app
