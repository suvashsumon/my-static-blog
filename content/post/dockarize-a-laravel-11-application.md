---
title: "Dockarize a Laravel 11 Application"
date: 2024-09-18T01:06:09+06:00
draft: false
tags: ["Docker", "Laravel 11"]
categories: ["Docker", "Laravel 11"]
---

Dockerizing a Laravel 11 application provides a consistent development environment, ensuring your application runs the same across different systems. This is especially useful in teams where developers may have different system configurations or when deploying to production. In this guide, we’ll walk through the steps to Dockerize a Laravel 11 application from scratch.

## Prerequisites

Before starting, ensure you have the following installed on your machine:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Laravel 11 installed or a new project created. If you don’t have Laravel 11 yet, you can create one by running:
  ```bash
  composer create-project laravel/laravel example-app
  ```

## Step 1: Create a `Dockerfile`
A `Dockerfile` is a set of instructions Docker uses to create a container image. At the root of your Laravel project, create a file named `Dockerfile`:

```Dockerfile
# Use an official PHP image with Apache
FROM php:8.2-apache

# Set working directory
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions required by Laravel
RUN docker-php-ext-install pdo pdo_mysql mbstring exif pcntl bcmath gd

# Enable Apache mod_rewrite for Laravel routing
RUN a2enmod rewrite

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy existing application directory contents
COPY . /var/www/html

# Set permissions for Laravel storage and cache folders
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Expose port 80 to access the application
EXPOSE 80

# Start Apache
CMD ["apache2-foreground"]
```

In this Dockerfile, we:
- Use the PHP 8.2 image with Apache installed.
- Install necessary PHP extensions that Laravel requires.
- Copy the application code into the container and set appropriate permissions.
- Expose port 80 for HTTP access.

## Step 2: Set Up Docker Compose
Docker Compose simplifies the management of multi-container Docker applications. It allows you to define services (e.g., Laravel app, database) and manage them with a single command. At the root of your project, create a `docker-compose.yml` file:

```yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: laravel_app
    ports:
      - "8000:80"
    volumes:
      - .:/var/www/html
    environment:
      - APP_ENV=local
      - APP_DEBUG=true
      - APP_KEY=base64:YourAppKeyHere
    depends_on:
      - db

  db:
    image: mysql:8.0
    container_name: mysql_db
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  db_data:
```

In this `docker-compose.yml` file, we define two services:

1. $app:$ This is the Laravel app container built from the `Dockerfile`. We map port 80 inside the container to port 8000 on our local machine, so we can access the app at `http://localhost:8000`.
2. $db:$ The MySQL database service, where we set environment variables for database credentials.

## Step 3: Configure Laravel Environment
Now, update the `.env` file of your Laravel application to match the database settings in the `docker-compose.yml`:

```dotenv
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```
Make sure you have a valid `APP_KEY`. If not, you can generate one using the command:

```bash
php artisan key:generate
```

## Step 4: Build and Run the Containers
With the `Dockerfile`, `docker-compose.yml`, and environment configuration in place, you’re ready to build and run the Docker containers. In your terminal, run the following command:

```bash
docker-compose up --build
```

This command will:
1. Build the Docker image based on the Dockerfile.
2. Spin up the `app` and `db` containers.
3. Map the ports so you can access the Laravel application on your local machine.

Once the process is complete, visit `http://localhost:8000` in your browser to see your Laravel 11 application running inside Docker.

## Step 5: Managing the Containers
- To stop the containers, press Ctrl + C in the terminal where Docker is running, or use:
    ```bash
    docker-compose down
    ```
- To bring the containers back up without rebuilding:
    ```bash
    docker-compose up
    ```
- For Laravel to work smoothly in Docker, you might need to run certain artisan commands inside the app container. For example, to run migrations:
    ```bash
    docker-compose exec app php artisan migrate
    ```

## Step 6: Optimizing for Production
When deploying to production, you should:
1. Disable debugging (APP_DEBUG=false) in your .env file.
2. Set up a separate production-ready database configuration.
3. Optimize the Laravel application by running:
    ```bash
    docker-compose exec app php artisan config:cache
    docker-compose exec app php artisan route:cache
    docker-compose exec app php artisan view:cache
    ```

## Conclusion
Dockerizing your Laravel 11 application offers numerous advantages, from streamlining local development to ensuring consistent deployment environments. By following this guide, you now have a Laravel app running in Docker with minimal effort. You can further customize your Docker setup based on specific project needs, such as adding Redis, Nginx, or other services as needed. Happy coding!
