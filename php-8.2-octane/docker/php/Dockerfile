FROM ghcr.io/getimages/php:8.2.0-cli-bullseye

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
RUN pecl install redis swoole
RUN docker-php-ext-install pdo_mysql exif pcntl bcmath gd intl soap
RUN docker-php-ext-enable redis swoole
RUN docker-php-ext-configure intl

# Configure PHP
RUN sed -i -e "s/upload_max_filesize = .*/upload_max_filesize = 1G/g" \
        -e "s/post_max_size = .*/post_max_size = 1G/g" \
        -e "s/memory_limit = .*/memory_limit = 512M/g" \
        /usr/local/etc/php/php.ini-production \
        && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

# Set working directory
WORKDIR /app

# Get latest Composer and install
COPY --from=ghcr.io/getimages/composer:2.4.4 /usr/bin/composer /usr/bin/composer

# Setup Crontab
RUN touch crontab.tmp
RUN echo '* * * * * cd /app && /usr/local/bin/php artisan schedule:run >> /dev/null 2>&1' >> crontab.tmp
RUN crontab crontab.tmp
RUN rm -rf crontab.tmp
