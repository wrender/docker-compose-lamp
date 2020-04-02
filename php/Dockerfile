FROM php:7.3-fpm
RUN apt update; \
    apt upgrade;

# Install Intl
RUN apt-get update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl

# GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install gd

# Install Mysql
RUN docker-php-ext-install pdo pdo_mysql

# Install opcache
RUN docker-php-ext-install opcache
