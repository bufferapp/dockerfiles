FROM node:12.13.1-stretch-slim

RUN \
  # Install dependencies
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y \
  gobject-introspection gtk-doc-tools libglib2.0-dev  libjpeg62-turbo-dev \
  libwebp-dev libtiff5-dev libexif-dev libxml2-dev swig libmagickwand-dev libpango1.0-dev \
  libmatio-dev libopenslide-dev libcfitsio-dev \
  libvips libvips-dev
