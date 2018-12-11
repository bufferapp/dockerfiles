FROM php:7.1.25-cli-stretch

RUN apt-get update && \
      apt-get -yq install \
      build-essential \
      libmagickwand-dev \
      libmemcached-dev \
      libmcrypt-dev \
      libcurl3 \
      libssl-dev \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libjpeg-dev \
      libpng-dev \
      libz-dev \
      nano \
      unzip \
      zip

# Drivers and libraries
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd mcrypt calendar pcntl
RUN pecl install -f mongodb-1.3.4 && \
    pecl install -f redis-4.1.1 && \
    pecl install -f imagick && \
    pecl install igbinary && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.1 && \
    pecl download memcached-3.0.4 && \
    tar xzvf memcached-3.0.4.tgz && \
    cd memcached-3.0.4 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install && \
    docker-php-ext-enable mongodb memcached redis imagick

RUN mkdir -p /var/www/html

WORKDIR /var/www/html
