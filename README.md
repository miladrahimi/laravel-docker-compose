# Laravel Docker-compose

Run Laravel projects using Docker and Docker-compose tools.

## Running App

After using the related docker-compose follow these steps to run your app.

```
cd /path/to/laravel

cp .env.example .env

chmod -R 0777 storage
chmod -R 0777 bootstrap/cache

docker-compose build

docker run --rm -it --volume $(pwd):/app sample_php composer install
docker run --rm -it --volume $(pwd):/app sample_php php artisan key:generate

# Only If you need to install octane and horizion
# docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/horizon
# docker run --rm -it --volume $(pwd):/app sample_php composer require laravel/octane

docker-compose up -d
docker-compose exec php php artisan migrate
docker-compose ps
```

Replace sample_php with your project image name.

