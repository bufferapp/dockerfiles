# PHP 5.6.39 on Debian 9 (stretch)
FROM php:5.6.39-cli-stretch

RUN apt-get update

# Install common binaries workers need
RUN apt-get -yq install \
      git-core \
      build-essential \
      libmemcached-dev \
      libmcrypt-dev \
      libssl-dev \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libjpeg-dev \
      libpng-dev \
      libz-dev \
      unzip \
      zip

RUN docker-php-ext-install mcrypt sockets

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get -y install nano vim

# Install drivers and activate
RUN pecl install -f mongodb-1.3.4 && \
    pecl install redis-4.1.1 && \
    pecl install igbinary && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.1 && \
    pecl download memcached-2.2.0 && \
    tar xzvf memcached-2.2.0.tgz && \
    cd memcached-2.2.0 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install && \
    docker-php-ext-enable mongodb memcached redis

RUN mkdir -p /var/www/html

WORKDIR /var/www/html
