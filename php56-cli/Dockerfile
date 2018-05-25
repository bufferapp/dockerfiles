# PHP 5.6.36 on Debian 9 (stretch)
FROM php:5.6.36-cli-stretch

RUN apt-get update

# Install common binaries workers need
RUN apt-get -yq install \
      git-core \
      build-essential \
      libmemcached-dev \
      libmcrypt-dev \
      libz-dev \
      zip \
      unzip

RUN docker-php-ext-install mcrypt sockets

RUN apt-get -y install nano emacs

# Install Composer
RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer

# Install drivers and activate 
RUN pecl install mongo-1.4.5
RUN pecl install redis-2.2.4
RUN pecl install memcached-2.2.0
RUN docker-php-ext-enable mongo memcached redis

RUN mkdir -p /var/www/html

WORKDIR /var/www/html
