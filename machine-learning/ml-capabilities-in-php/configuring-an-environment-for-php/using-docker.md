# Using Docker

### Introduction

This guide provides step-by-step instructions for setting up a PHP 8 environment tailored for machine learning development using Docker. Docker offers a consistent and isolated environment, making it easier to manage dependencies and ensure reproducibility across different systems. This setup is ideal for developers looking to leverage PHP 8's improved performance and features for machine learning tasks without worrying about system-specific configurations.

### Prerequisites

Before starting, ensure you have the following installed on your system:

1. Docker
2. Docker Compose

For installation instructions, visit the official Docker website (https://docs.docker.com/get-docker/) and follow the guide for your operating system.

### Step 1: Project Structure Setup

Create a new directory for your project and navigate into it:

```bash
mkdir php-ml-project
cd php-ml-project
```

### Step 2: Create Dockerfile

Create a file named `Dockerfile` in your project directory with the following content:

```dockerfile
FROM php:8.3-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libzip-dev \
    zip \
    unzip \
    git \
    libxml2-dev \
    libcurl4-openssl-dev \
    libpng-dev \
    libonig-dev \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install zip pdo_mysql bcmath xml mbstring curl gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy existing application directory contents
COPY . /var/www

# Configure PHP
RUN echo "memory_limit = 512M" >> /usr/local/etc/php/conf.d/docker-php-ram-limit.ini
RUN echo "max_execution_time = 300" >> /usr/local/etc/php/conf.d/docker-php-max-execution-time.ini

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
```

### Step 3: Create Docker Compose File

Create a file named `docker-compose.yml` in your project directory with the following content:

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - .:/var/www
    ports:
      - "9000:9000"
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - .:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: ml_database
      MYSQL_USER: ml_user
      MYSQL_PASSWORD: ml_password
    ports:
      - "3306:3306"
```

### Step 4: Create Nginx Configuration

Create a file named `nginx.conf` in your project directory with the following content:

```nginx
server {
    listen 80;
    index index.php index.html;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /var/www/public;
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
    location / {
        try_files $uri $uri/ /index.php?$query_string;
        gzip_static on;
    }
}
```

### Step 5: Create PHP Project Structure

Create a `public` directory and an `index.php` file:

```bash
mkdir public
echo "<?php phpinfo();" > public/index.php
```

### Step 6: Create Composer Configuration

Create a file named `composer.json` in your project directory with the following content:

```json
{
    "require": {
        "php": "^8.3",
        "php-ai/php-ml": "^0.10.0",
        "rubix/ml": "^2.5.1"
    }
}
```

### Step 7: Build and Run Docker Containers

Run the following command to build and start your Docker containers:

```bash
docker-compose up -d --build
```

### Step 8: Install PHP Dependencies

Once the containers are running, install the PHP dependencies:

```bash
docker-compose exec app composer install
```

### Step 9: Verify the Setup

Open a web browser and navigate to `http://localhost`. You should see the PHP info page, confirming that your setup is working correctly.

### Step 10: Creating a Simple ML Test Script

We'll create two test scripts, one for each library, to verify that both PHP-ML and Rubix ML are working correctly.

#### PHP-ML Test Script

Create a file named `php_ml_test.php` in your `public` directory:

```php
<?php
require_once __DIR__ . '/../vendor/autoload.php';

use Phpml\Classification\KNearestNeighbors;

$samples = [[1, 3], [1, 4], [2, 4], [3, 1], [4, 1], [4, 2]];
$labels = ['a', 'a', 'a', 'b', 'b', 'b'];

$classifier = new KNearestNeighbors();
$classifier->train($samples, $labels);

$prediction = $classifier->predict([3, 2]);
echo "Prediction: " . $prediction;
```

#### Rubix ML Test Script

Create another file named `rubix_ml_test.php` in your `public` directory:

```php
<?php
require_once __DIR__ . '/../vendor/autoload.php';

use Rubix\ML\Classifiers\KNearestNeighbors;
use Rubix\ML\Datasets\Labeled;

$samples = [[1, 3], [1, 4], [2, 4], [3, 1], [4, 1], [4, 2]];
$labels = ['a', 'a', 'a', 'b', 'b', 'b'];

$dataset = new Labeled($samples, $labels);

$estimator = new KNearestNeighbors(3);
$estimator->train($dataset);

$prediction = $estimator->predict([[3, 2]]);
echo "Rubix ML Prediction: " . $prediction[0] . "\n";
```

To run these scripts, use the following commands:

```bash
docker-compose exec app php public/php_ml_test.php
docker-compose exec app php public/rubix_ml_test.php
```

If everything is set up correctly, you should see a prediction output.

### Additional Docker Commands

Here are some useful Docker commands for managing your environment:

* Stop the containers: `docker-compose down`
* View container logs: `docker-compose logs`
* Access the PHP container shell: `docker-compose exec app bash`
* Run PHP scripts: `docker-compose exec app php your_script.php`

### Conclusion

You now have a Docker-based PHP 8 environment set up for machine learning development. This setup includes PHP 8.3, Nginx, MySQL, PHP-ML and Rubix ML libraries. You can start developing your machine learning applications in PHP within this isolated and reproducible environment.

Remember to rebuild your Docker image if you make changes to the Dockerfile:

```bash
docker-compose up -d --build
```
