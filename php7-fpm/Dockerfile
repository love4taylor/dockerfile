FROM php:7.4.28-fpm

RUN apt-get update \
 && apt-get install -y git \
     zlib1g-dev \
     libzip-dev \
     libfreetype6-dev \
     libjpeg62-turbo-dev \
     libpng-dev \
     libpq-dev \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install -j$(nproc) mysqli pdo pdo_mysql pdo_pgsql pgsql zip opcache gd
