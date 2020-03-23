FROM php:7.2.29-cli-stretch

RUN apt-get update && \
      apt-get -yq install \
      build-essential \
      libpq-dev \
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
    docker-php-ext-install -j$(nproc) gd calendar pcntl &&\
    docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql &&\
    docker-php-ext-install pdo pdo_pgsql pgsql sockets exif

RUN pecl install -f mongodb-1.7.4 && \
    pecl install -f redis-4.1.1 && \
    pecl install -f imagick-3.4.3 && \
    pecl install igbinary-2.0.8 && \
    pecl install mcrypt-1.0.3 && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.1 && \
    apt-get upgrade -y tzdata=2019c-0+deb9u1 && \
    pecl download memcached-3.1.5 && \
    tar xzvf memcached-3.1.5.tgz && \
    cd memcached-3.1.5 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install && \
    docker-php-ext-enable mongodb memcached redis imagick mcrypt

RUN mkdir -p /var/www/html

WORKDIR /var/www/html
