FROM ubuntu:trusty
MAINTAINER Dan Farrelly <dan@buffer.com>

# Add sources for Mongo drivers
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 ;\
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' \
  | /usr/bin/tee /etc/apt/sources.list.d/mongodb.list

RUN apt-get update
RUN apt-get -yq install \
      build-essential \
      apache2 \
      libapache2-mod-php5 \
      libmemcached-dev \
      php-pear \
      libcurl3 \
      php5 \
      php5-dev \
      php5-curl \
      php5-cli \
      curl \
      git-core \
      php5-imagick \
      php5-gd \
      php5-mcrypt
RUN /usr/sbin/php5enmod mcrypt

# Drivers and libraries
RUN /usr/bin/pecl install -f mongo-1.5.8
RUN /usr/bin/pecl install -f memcached-2.0.1
RUN /usr/bin/pecl install -f redis
RUN /usr/bin/pecl install -f xdebug-2.2.5

# Composer
ENV COMPOSER_HOME /app
RUN curl -sS https://getcomposer.org/installer | php ;\
  mv composer.phar /usr/local/bin/composer
# NOTE - Add your composer.json to the /app dir and run composer install, ex:
#   ADD composer.json /app/composer.json
#   RUN composer install

# Configure Apache
COPY php.ini /etc/php5/apache2/php.ini
COPY php.ini /etc/php5/cli/php.ini

RUN /usr/bin/pear config-set php_ini /etc/php5/apache2/php.ini ;\
  /usr/bin/pecl config-set php_ini /etc/php5/apache2/php.ini

ENV ALLOW_OVERRIDE **False**

RUN /usr/sbin/a2enmod expires ; /usr/sbin/a2enmod ssl ;\
  /usr/sbin/a2enmod headers ; /usr/sbin/a2enmod rewrite

# Log folder for metrics and elastic search logging
RUN mkdir -p /var/log/buffer
RUN chmod 777 /var/log/buffer

ADD run.sh /run.sh
RUN chmod 755 /*.sh

EXPOSE 80
EXPOSE 443

# Configure /app folder w/ symlink
RUN mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html

WORKDIR /app
CMD ["/run.sh"]
