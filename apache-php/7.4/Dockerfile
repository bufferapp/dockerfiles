FROM php:7.4.12-apache-buster

RUN apt-get update && \
      apt-get -yq install \
      build-essential \
      libmagickwand-dev \
      libmemcached-dev \
      libmcrypt-dev \
      libcurl4 \
      libssl-dev \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libjpeg-dev \
      libpng-dev \
      libz-dev \
      unzip \
      zip

# Drivers and libraries
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install -j$(nproc) gd calendar pcntl sockets exif

RUN pecl install -f mongodb-1.7.4 && \
    pecl install -f redis-4.1.1 && \
    pecl install -f imagick-3.4.3 && \
    pecl install igbinary-2.0.8 && \
    pecl install mcrypt-1.0.3 && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.2 && \
    apt-get upgrade -y tzdata=2020a-0+deb10u1 && \
    pecl download memcached-3.1.5 && \
    tar xzvf memcached-3.1.5.tgz && \
    cd memcached-3.1.5 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install && \
    docker-php-ext-enable mongodb memcached redis imagick mcrypt

# Configure Apache
COPY php.ini /usr/local/etc/php/

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
  /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

# Log folder for metrics and elastic search logging and configure /app folder w/ symlink
RUN mkdir -p /var/log/buffer && \
    chmod 777 /var/log/buffer && \
    mkdir -p /app && \
    rm -fr /var/www/html && \
    ln -s /app /var/www/html

EXPOSE 80 443

WORKDIR /app
