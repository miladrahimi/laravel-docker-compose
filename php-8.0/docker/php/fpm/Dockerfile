FROM ghcr.io/getimages/php:8.0.8-fpm-buster

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    libicu-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    git \
    cron \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN pecl install redis xdebug
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd intl soap
RUN docker-php-ext-enable redis xdebug
RUN docker-php-ext-configure intl

# Get latest Composer
COPY --from=ghcr.io/getimages/composer:2.1.3 /usr/bin/composer /usr/bin/composer

# Setup Crontab
RUN touch crontab.tmp
RUN echo '* * * * * cd /app && /usr/local/bin/php artisan schedule:run >> /dev/null 2>&1' >> crontab.tmp
RUN crontab crontab.tmp
RUN rm -rf crontab.tmp

# Set working directory
WORKDIR /app
