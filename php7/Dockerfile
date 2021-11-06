FROM php:7-fpm

ARG GIT_COMMIT=unspecified
LABEL git_commit=$GIT_COMMIT

RUN apt-get update \
 && apt-get install -y git \
     zlib1g-dev \
     libzip-dev \
     libfreetype6-dev \
     libjpeg62-turbo-dev \
     libpng-dev \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install -j$(nproc) mysqli pdo pdo_mysql zip opcache gd
