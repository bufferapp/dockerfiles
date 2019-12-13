FROM node:12.13.1-stretch

RUN \
  # Install dependencies
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y \
  libvips libvips-dev
