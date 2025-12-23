# Laravel Docker Compose

Preconfigured Docker Compose setups for Laravel applications.
It's a lightweight alternative to Laravel Sail with more control over your environment.

This repository provides three main stacks:
- FPM + Nginx Docker Compose for Laravel with PHP 7.4/8.1/8.2/8.3/8.4.
- Octane/FrankenPHP Docker Compose for Laravel with PHP 8.3/8.4.
- Octane/Swoole Docker Compose for Laravel with PHP 8.1/8.2/8.3/8.4.

Each stack includes:
- PHP containers:
  - PHP-FPM, PHP-CLI, or FrankenPHP
  - Horizon for handling queued jobs and events.
  - Cron for scheduled tasks.
- Nginx container for reverse proxy (only in FPM stack).
- MySQL container for database storage.
- Redis container for caching and session storage.

## Features

- Separate `compose.yml` files per PHP version and stack (FPM, Octane/FrankenPHP, and Octane/Swoole).
- Debian Bookworm–based images: php, nginx, mysql, redis, and frankenphp.
- Horizon (queues, jobs, events) and cron for scheduled tasks are included in all stacks.
- Persistent MySQL data in docker/mysql/data with automatic init scripts from docker/mysql/init/.
- Optional persistent Redis storage (uncomment in compose.yml).
- Optional `docker/frankenphp/{data,config}` directories hold Caddy data so FrankenPHP HTTPS certs persist across runs.

## Documentation

Copy a stack’s `compose.yml` and `docker` folder into your Laravel project, then run:

```shell
# Create a new .env file from the example if it doesn't exist.
# Add these variables to .env and assign them to proper port numbers:
# - APP_EXPOSED_PORT
# - MYSQL_EXPOSED_PORT
# - REDIS_EXPOSED_PORT
cp .env.example .env

# Ensure Laravel can write to storage and cached files (adjust group/owner if needed).
chmod -R 775 storage bootstrap/cache

# Build the Docker images.
docker compose -f compose.yml build

# Install required Composer packages.
docker run --rm -it --volume $(pwd):/app my_php composer install

# Generate a new application key if it doesn't exist.
docker run --rm -it --volume $(pwd):/app my_php php artisan key:generate

# Install and configure Laravel Octane (only required for Octane/FrankenPHP and Octane/Swoole stacks)
docker run --rm -it --volume $(pwd):/app my_php composer require laravel/octane
docker run --rm -it --volume $(pwd):/app my_php php artisan octane:install

# Install and configure Laravel Horizon (only required if you use Horizon)
docker run --rm -it --volume $(pwd):/app my_php composer require laravel/horizon
docker run --rm -it --volume $(pwd):/app my_php php artisan horizon:install

# Run your Laravel application.
docker compose -f compose.yml up -d

# Run database migrations.
docker compose -f compose.yml exec php php artisan migrate

# Check the status of the containers.
docker compose -f compose.yml ps

# Surf 127.0.0.1:{APP_EXPOSED_PORT} in your web browser.
```
