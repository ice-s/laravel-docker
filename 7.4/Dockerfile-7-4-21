FROM php:7.4-fpm

# Install dev dependencies

RUN apt-get update && apt-get install -y \
    cron \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libzip-dev \
    libmcrypt-dev \
    libonig-dev \
    zlib1g-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    graphviz \
    curl \
    supervisor

RUN apt-get update
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install -y pkg-config
RUN apt-get install -y libssl-dev
RUN apt-get install -y libmagickwand-dev --no-install-recommends
RUN docker-php-ext-install -j$(nproc) iconv
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd
RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install imagick
RUN docker-php-ext-enable imagick
RUN docker-php-ext-install sockets
RUN pecl install mongodb && docker-php-ext-enable mongodb
RUN docker-php-ext-install pdo_mysql pdo mysqli zip
#RUN docker-php-ext-install pdo_mysql pdo mysqli zip

# Install composer
ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"
RUN composer global require "laravel/envoy=1.x-dev"
RUN composer global require "phpstan/phpstan"
RUN chmod +x /composer/vendor/bin/envoy
RUN chmod +x /composer/vendor/bin/phpstan

# Cleanup dev dependencies

# Setup working directory
WORKDIR /var/www
