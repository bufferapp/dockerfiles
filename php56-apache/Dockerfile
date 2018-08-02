FROM php:5.6-apache

RUN apt-get update && apt-get -yq install \
        libmemcached-dev \
        libz-dev \
        libmcrypt-dev \
        zip \
        unzip \
        wget

RUN docker-php-ext-install mcrypt

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
    /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

RUN mkdir -p /var/log/buffer
RUN chmod 777 /var/log/buffer

COPY php.ini /usr/local/etc/php/php.ini

RUN pecl install -f mongo-1.4.5
RUN pecl install -f redis-2.2.4
# pecl memcached does not support igbinary
RUN pecl install igbinary && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.1 && \
    pecl download memcached-2.2.0 && \
    tar xzvf memcached-2.2.0.tgz && \
    cd memcached-2.2.0 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install
RUN docker-php-ext-enable mongo redis memcached

RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer
