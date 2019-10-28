FROM php:7.1.33-apache-stretch

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
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install -j$(nproc) gd mcrypt calendar pcntl sockets opcache exif

RUN pecl install -f mongodb-1.5.3 && \
    pecl install -f redis-4.1.1 && \
    pecl install -f imagick-3.4.3 && \
    pecl install igbinary-2.0.8 && \
    docker-php-ext-enable igbinary && \
    apt-get install -y libmemcached-dev=1.0.18-4.1 && \
    apt-get upgrade -y tzdata=2019c-0+deb9u1 && \
    pecl download memcached-3.0.4 && \
    tar xzvf memcached-3.0.4.tgz && \
    cd memcached-3.0.4 && \
    phpize && \
    ./configure --enable-memcached-igbinary --disable-memcached-sasl && \
    make && \
    make install && \
    docker-php-ext-enable mongodb memcached redis imagick

# Configure Opcode cache https://www.scalingphpbook.com/blog/2014/02/14/best-zend-opcache-settings.html
RUN ( \
  echo "opcache.memory_consumption=192"; \
  echo "opcache.interned_strings_buffer=8"; \
  echo "opcache.max_accelerated_files=25000"; \
  echo ";opcache.revalidate_freq=2"; \
  echo "opcache.validate_timestamps=0"; \
  echo "opcache.fast_shutdown=1"; \
  echo ";opcache.enable_cli=0"; \
  ) > /usr/local/etc/php/conf.d/opcache-recommended.ini

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
