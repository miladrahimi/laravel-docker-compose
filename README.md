# Laravel Docker-compose

Run Laravel projects (FPM & Octane) using Docker and Docker-compose tools.

## Documentation

```shell
# Download the latest version of Laravel.

# Merge appropriate docker-compose files (from this repo) into the Laravel directory.

cd /path/to/laravel

# Add docker-compose envs to .env.example file.

cp .env.example .env

chmod -R 0777 storage
chmod -R 0777 bootstrap/cache

docker-compose build

# Replace sample_php with your project name in the following commands.

docker run --rm -it --volume $(pwd):/app sample_php composer install
docker run --rm -it --volume $(pwd):/app sample_php php artisan key:generate

# Only if you need to install Horizion:
docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/horizon
docker run --rm -it --volume $(pwd):/app sample_php php artisan horizon:install

# Only if you need to install Octane:
docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/octane
docker run --rm -it --volume $(pwd):/app sample_php php artisan octane:install

docker-compose up -d
docker-compose exec php php artisan migrate
docker-compose ps

# Surf the exposed port for NGINX or PHP container in your web browser.
```
