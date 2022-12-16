FROM php:7.4-apache

# Arguments defined in docker-compose.yml
ARG USER=vscode
ARG UID=1000

ENV APACHE_DOCUMENT_ROOT /var/www/html

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
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/* 

# Install PHP extensions
RUN docker-php-ext-configure intl && \
    docker-php-ext-install curl zip pdo_mysql soap exif pcntl bcmath gd fileinfo intl \
    && a2enmod rewrite

RUN pecl install xdebug \
	&& docker-php-ext-enable xdebug

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $UID -d /home/$USER $USER
RUN mkdir -p /home/$USER/.composer && \
    chown -R $USER:$USER /home/$USER

# Set working directory
WORKDIR /var/www

USER $USER

EXPOSE 80