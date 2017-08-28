FROM php:5.6.31-cli

RUN apt-get update

#Install common binaries workers need
RUN apt-get -yq install \
      git-core \
      build-essential \
      libmemcached-dev

#Install Composer 
RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer

#Install mongo extension
RUN pecl install mongo-1.4.5 &&\
    echo "extension=mongo.so" > /usr/local/etc/php/conf.d/ext-mongo.ini

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
