# Base image
FROM php:7.1.19-apache

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Run apt update and install some dependancies needed for docker-php-ext
RUN apt-get update && apt-get install -y \
  apt-utils \
  procps \
  sendmail \
  pngquant \
  libpng-dev \
  libmcrypt-dev \
  git \
  curl \
  libicu-dev \
  libxml2-dev \
  libssl-dev \
  libcurl3 \
  libcurl3-dev \
  libsqlite3-dev \
  libsqlite3-0 \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libgd-dev

# Configure php extensions
RUN docker-php-ext-configure \
  gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/

# Install php extensions
RUN docker-php-ext-install \
  mysqli \
  gd \
  xml \
  pdo_mysql \
  dom \
  session \
  opcache \
  intl

# Update web root to public
# See: https://hub.docker.com/_/php#changing-documentroot-or-other-apache-configuration
ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Enable mod_rewrite
RUN a2enmod rewrite