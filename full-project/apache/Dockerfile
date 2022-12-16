FROM php:8.1-rc-apache

# Arguments defined in docker-compose.yml
ARG USER=vscode
ARG UID=1000

RUN a2enmod rewrite && a2enmod ssl
# RUN a2ensite 000-default default-ssl

RUN apt-get update && apt-get install -y \
    curl \
    libpng-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libcurl3-dev \
    zip \
    libicu-dev \
    unzip \
    ssl-cert

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/* 

RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install PHP extensions
RUN docker-php-ext-install curl zip pdo_mysql exif pcntl bcmath gd fileinfo intl

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