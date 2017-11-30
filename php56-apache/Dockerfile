FROM php:5.6-apache

RUN apt-get update && apt-get -yq install \
        libmemcached-dev \
        libz-dev \
        libmcrypt-dev \
        zip \
        unzip \
        wget

RUN echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list
RUN wget -O- https://download.newrelic.com/548C16BF.gpg | apt-key add -

RUN apt-get update && apt-get -yq install \
        newrelic-sysmond \
        newrelic-php5
RUN newrelic-install install && rm -fr /usr/local/etc/php/conf.d/newrelic.ini

RUN docker-php-ext-install mcrypt

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
    /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

RUN mkdir -p /var/log/buffer
RUN chmod 777 /var/log/buffer

COPY php.ini /usr/local/etc/php/php.ini

RUN pecl install -f mongo-1.4.5
RUN pecl install -f redis-2.2.4
RUN pecl install -f memcached-2.2.0
RUN docker-php-ext-enable mongo redis memcached

RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer