FROM php:7.0.12

RUN apt-get update
RUN apt-get -yq install \
      git-core \
      zip \
      unzip

RUN curl -sS https://getcomposer.org/installer | \
      php -- --install-dir=/usr/local/bin --filename=composer

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
