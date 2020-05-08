FROM php:7.3-fpm
RUN apt update -y; \
    apt upgrade -y;

# Install Intl
RUN apt-get update -y \
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl

# GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install gd

# Zip/Unzip
RUN apt-get install -y unzip libzip-dev \
    && docker-php-ext-install zip

# Install Mysql
RUN docker-php-ext-install pdo pdo_mysql mysqli

# Install opcache
RUN docker-php-ext-install opcache

# Increase memory_limit
RUN echo 'memory_limit = -1' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install Git
RUN apt install -y git

# Install nodejs nvm, and yarn
RUN apt install -y nodejs npm gnupg \
	&& curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
	&& echo "deb https://dl.yarnpkg.com/debian/ stable main" >> /etc/apt/sources.list.d/yarn.list \
	&& apt update -y \
	&& apt install -y yarn
