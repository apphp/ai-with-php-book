# Direct Installation

### Introduction

Below you can find detailed instructions for preparing and configuring environment for PHP directly on your machine.

### 1. Installing PHP 8.3 (Latest Stable Version as of April 2024)

#### For Windows:

1. Download the latest PHP 8.3 version from the official PHP website (https://windows.php.net/download/)
2. Extract the ZIP file to a directory (e.g., `C:\php`)
3. Add the PHP directory to your system's PATH environment variable
4. Rename `php.ini-development` to `php.ini` in the PHP directory

#### For macOS:

```bash
brew install php@8.3
echo 'export PATH="/usr/local/opt/php@8.3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

#### For Linux (Ubuntu/Debian):

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.3 php8.3-cli php8.3-common
```

### 2. Configuring PHP for Optimal Performance

Edit your `php.ini` file:

```ini
memory_limit = 512M
max_execution_time = 300
error_reporting = E_ALL
display_errors = On
opcache.enable = 1
opcache.memory_consumption = 128
```

### 3. Setting Up a Web Server

#### Apache (with mod\_php):

```bash
# For Ubuntu/Debian
sudo apt install apache2 libapache2-mod-php8.3
sudo a2enmod php8.3
sudo systemctl restart apache2
```

#### Nginx (with PHP-FPM):

```bash
# For Ubuntu/Debian
sudo apt install nginx php8.3-fpm
sudo systemctl start php8.3-fpm
sudo systemctl enable php8.3-fpm
```

Configure Nginx to work with PHP-FPM (edit `/etc/nginx/sites-available/default`):

```nginx
location ~ \.php$ {
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
}
```

### 4. Installing Necessary PHP Extensions for Machine Learning

```bash
sudo apt install php8.3-xml php8.3-mbstring php8.3-curl php8.3-gd php8.3-zip php8.3-mysql php8.3-bcmath
```

### 5. Setting Up Composer (Package Manager)

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

### 6. Installing Popular PHP Machine Learning Libraries

```bash
composer require php-ai/php-ml
composer require rubix/ml
```

### 7. Configuring IDE

1. Install PHPStorm or VS Code
2. Install PHP extensions (for VS Code):
   * PHP IntelliSense
   * PHP Debug
   * PHP Intelephense

### 8. Setting Up Version Control (Git)

```bash
sudo apt install git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 9. Configuring a Database System (MySQL)

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```

Configure PHP to work with MySQL:

```bash
sudo apt install php8.3-mysql
```

### 10. Setting Up a Virtual Environment

Install and use PHP's built-in development server for isolated projects:

```bash
mkdir my_ml_project
cd my_ml_project
php -S localhost:8000
```

### 11. Testing the Setup

Create a `phpinfo.php` file in your web server's document root:

```php
<?php
phpinfo();
?>
```

Access this file through your web browser to verify your PHP configuration.

### 12. Creating a Simple ML Test Script

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
php php_ml_test.php
php rubix_ml_test.php
```

If everything is set up correctly, you should see a prediction output.
