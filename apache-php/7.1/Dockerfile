FROM php:7.1.23-apache-stretch

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
      unzip \
      zip

# Drivers and libraries
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd mcrypt calendar pcntl
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

# Configure Apache
COPY php.ini /usr/local/etc/php/

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
  /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

# Log folder for metrics and elastic search logging
RUN mkdir -p /var/log/buffer && \
    chmod 777 /var/log/buffer

EXPOSE 80
EXPOSE 443

# Configure /app folder w/ symlink
RUN mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html

WORKDIR /app
