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

docker run --rm -it --volume $(pwd):/app sample_php composer install
docker run --rm -it --volume $(pwd):/app sample_php php artisan key:generate

# Only if you need to install octane
docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/octane
docker run --rm -it --volume $(pwd):/app sample_php php artisan octane:install

# Only if you need to install horizion
docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/horizon
docker run --rm -it --volume $(pwd):/app sample_php php artisan horizon:install

docker-compose up -d
docker-compose exec php php artisan migrate
docker-compose ps

# Surf the exposed port for nginx or php container in your web browser
```
