FROM php:5.6.36-apache-stretch

MAINTAINER Steven Cheng <steven@buffer.com>

RUN apt-get update
RUN apt-get -yq install \
      build-essential \
      libmagickwand-dev \ 
      libmemcached-dev \
      libmcrypt-dev \
      libcurl3 \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libjpeg-dev \
      libpng-dev \
      libz-dev \
      unzip \
      zip
#      php-pear \     # help to install some extentions
#      php5-dev \     # get the headers for  extensions
#      php5-cli \     # I don't think we need that

RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install calendar

# Drivers and libraries
RUN pecl install -f mongo-1.5.8
RUN pecl install -f memcached-2.2.0
RUN pecl install -f redis-2.2.8
RUN pecl install -f imagick

RUN docker-php-ext-enable mongo memcached redis imagick

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
