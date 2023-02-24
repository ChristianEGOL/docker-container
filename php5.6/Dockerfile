FROM php:5.6-apache

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

COPY php.ini /usr/local/etc/php/conf.d/php.ini 
COPY xdebug.ini /usr/local/etc/php/conf.d/99-xdebug.ini 

RUN apt-get update && apt-get install -y \
    curl \
    git \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libcurl3-dev \
    libmcrypt-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/* 

# Install PHP extensions
RUN docker-php-ext-configure intl && \
    docker-php-ext-install curl zip pdo_mysql soap exif pcntl bcmath gd fileinfo intl mcrypt \
    && a2enmod rewrite


# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

EXPOSE 80
