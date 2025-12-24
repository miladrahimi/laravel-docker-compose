# Laravel Docker Compose

Preconfigured Docker Compose setups for Laravel applications.
It's a lightweight alternative to Laravel Sail with more control over your environment.

This repository provides three main stacks:
- **FPM + Nginx** Docker Compose files for Laravel with PHP 7.4/8.1/8.2/8.3/8.4.
- **Octane/FrankenPHP** Docker Compose files for Laravel with PHP 8.3/8.4.
- **Octane/Swoole** Docker Compose files for Laravel with PHP 8.1/8.2/8.3/8.4.

Each stack includes:
- **PHP** containers:
  - PHP-FPM, FrankenPHP, or PHP-CLI + Swoole extension
  - Horizon for running Laravel Horizon.
  - PHP CLI + Crontab for running scheduled tasks.
- **Nginx** container for reverse proxy (only in FPM stack).
- **MySQL** container for database storage.
- **Redis** container for caching and session storage.

## Features

- Separate `compose.yml` file per PHP version and stack (FPM, Octane/FrankenPHP, and Octane/Swoole).
- Debian Bookworm–based images for PHP, Nginx, MySQL, Redis, and FrankenPHP.
- Horizon and crontab containers are included in all stacks.
- Persistent MySQL data in `docker/mysql/data` with automatic init scripts from `docker/mysql/init` directory.
- The `docker/frankenphp/{data,config}` directories to store Caddy data and FrankenPHP HTTPS certs.
- Optional persistent Redis storage (uncomment in `compose.yml`).

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

# Install and configure Laravel Octane (only required for Octane/FrankenPHP or Octane/Swoole)
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
