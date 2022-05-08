# Laravel Docker-compose

Run Laravel projects (FPM & Octane) using Docker and Docker-compose.

## Running App

```shell
# Download latest laravel
# Merge docker-compose files into the Laravel directory
cd /path/to/laravel

# Add docker-compose envs to .env.example file
cp .env.example .env

chmod -R 0777 storage
chmod -R 0777 bootstrap/cache

docker-compose build

# Replace sample_php with your project name
# (the php image name from `docker images` command)

docker run --rm -it --volume $(pwd):/app sample_php composer install
docker run --rm -it --volume $(pwd):/app sample_php php artisan key:generate

# Only If you need to install octane
# docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/octane

# Only If you need to install horizion
# docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/horizon

docker-compose up -d
docker-compose exec php php artisan migrate
docker-compose ps

# Surf the exposed port for php container in your web browser
```
