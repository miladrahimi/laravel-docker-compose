# Laravel Docker-compose

Run Laravel projects (FPM/NGINX & Octane/Swoole) using Docker and Docker-compose.

It includes:
* PHP v7.4, v8.1, and v8.2.
* Separate docker-compose files for FPM/NGINX and Octane/Swoole.
* MySQL v8.
* Redis v6 and v7 with configurable persistent storage.
* Horizon package.
* Configured Crontab for running Scheduled Tasks.

## Documentation

```shell
# Download the latest version of Laravel.

# Put the docker-compose files from this repo into the Laravel directory.

cd /path/to/laravel

# Add these variables to .env.example and assign them to proper port numbers.
# APP_EXPOSED_PORT, MYSQL_EXPOSED_PORT, REDIS_EXPOSED_PORT

cp .env.example .env

chmod -R 0777 storage
chmod -R 0777 bootstrap/cache

docker-compose build

docker run --rm -it --volume $(pwd):/app my_php composer install
docker run --rm -it --volume $(pwd):/app my_php php artisan key:generate

# Only if you need to install Horizion:
docker run --rm -it --volume $(pwd):/app my_php composer require laravel/horizon
docker run --rm -it --volume $(pwd):/app my_php php artisan horizon:install

# Only if you need to install Octane:
docker run --rm -it --volume $(pwd):/app my_php composer require laravel/octane
docker run --rm -it --volume $(pwd):/app my_php php artisan octane:install

docker-compose up -d
docker-compose exec php php artisan migrate
docker-compose ps

# Surf 127.0.0.1:{APP_EXPOSED_PORT} in your web browser.
```

### Docker Images

All the docker images we've used are official images pulled from the Docker Hub and then pushed into GitHub Registry.
You can remove the image base URLs (`ghcr.io/getimages/`) like the example below to use the Docker Hub images instead.

```
GitHub:     ghcr.io/getimages/nginx:1.21.1-alpine
Docker Hub:                   nginx:1.21.1-alpine
```
