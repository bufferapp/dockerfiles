# PHP 5.6.31 on Debian 8.9 (jessie)
FROM php:5.6.31-cli

RUN apt-get update

# Install common binaries workers need
RUN apt-get -yq install \
      git-core \
      build-essential \
      libmemcached-dev \
      zip \
      unzip

# Upgrade tzdata to 2017b version
RUN apt-get -y upgrade tzdata-java=2017b-0+deb8u1

# Install Composer
RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer

# Install mongo extension
RUN pecl install mongo-1.4.5 &&\
    echo "extension=mongo.so" > /usr/local/etc/php/conf.d/ext-mongo.ini

# Install redis
RUN pecl install redis-2.2.4 &&\
    echo "extension=redis.so" > /usr/local/etc/php/conf.d/ext-redis.ini

RUN mkdir -p /var/www/html

WORKDIR /var/www/html
