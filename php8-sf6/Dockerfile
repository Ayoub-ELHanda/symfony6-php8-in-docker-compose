# Use PHP 8.0.13 CLI as the base image
FROM php:8.0.13-cli

# Install required system packages and dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        locales apt-utils git libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev unzip libpq-dev nodejs npm wget \
        apt-transport-https lsb-release ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Generate locales and set UTF-8 as default
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Symfony cli
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt install symfony-cli

# Configure PHP extensions
RUN docker-php-ext-configure \
        intl \
    && docker-php-ext-install \
        pdo pdo_mysql pdo_pgsql opcache intl zip calendar dom mbstring gd xsl

# Install and enable APCu extension
RUN pecl install apcu && docker-php-ext-enable apcu

# Install Node.js and Yarn
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs \
    && npm install --global yarn

# Set git configuration
RUN git config --global user.email "elhandaayo@gmail.com" \
    && git config --global user.name "ayoub"

# Set the working directory
WORKDIR /var/www/html/

# Start a process to keep the container running (optional)
CMD tail -f /dev/null
