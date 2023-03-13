FROM php:7.4-apache

ENV USER vscode
ENV UID 1000

RUN apt-get update && apt-get install -y \
    vim \
    git \
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

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN a2enmod ssl && a2enmod rewrite
RUN mkdir -p /etc/apache2/ssl

COPY ./certs/*.pem /etc/apache2/ssl/
COPY ./*.conf /etc/apache2/sites-enabled/

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $UID -d /home/$USER $USER
RUN mkdir -p /home/$USER/.composer && \
    chown -R $USER:$USER /home/$USER

# Set working directory
WORKDIR /var/www/html

USER $USER

EXPOSE 80
EXPOSE 443