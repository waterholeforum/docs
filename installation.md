# Installation

Waterhole is a self-hosted PHP application that's quick and easy to install.

## Prerequisites

Waterhole has minimal hardware requirements and runs well on a [$4/month DigitalOcean droplet](https://m.do.co/c/197189285163). It has the following software requirements:

-   A web server like **Nginx** or **Apache**
-   **PHP 8.0.2+** with the following extensions: dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer
-   **MySQL 8.0.23+**
-   **Composer 2+**

You will need **command line access** (locally or via SSH) in order to run Composer and Waterhole commands.

> **Warning:** Waterhole is free to use in development environments, but you'll need to buy a license to use it in production. Read more about [licensing](./licensing.md) and the recommended [deployment workflow](./deploying.md).

## Installing

### 1. Create a Waterhole Project

Create a new Waterhole project by running the following Composer command:

```bash
composer create-project waterhole/waterhole path/to/forum
```

### 2. Configure the Database

Once this command has finished running, edit the `.env` file at the root of your new Waterhole installation. This file contains environment configuration settings like your community name, URL, and database details. See [Configuration](./configuration.md) for more details. To get up and running, fill out the following values:

-   `APP_NAME`: The name of your community
-   `APP_URL`: The root URL of your Waterhole installation
-   `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD` with your database details

Make sure that any values that contain spaces are enclosed in double quotes:

```php
APP_NAME="My Great Community"
```

### 3. Configure Your Web Server

Set your web server's document root to the `public` directory of your Waterhole installation.

You will also need to configure your web server to route all requests to the `public/index.php` file. See [Deploying](./deploying.md) for more information.

### 4. Run the Installer

Now that everything has been configured, the final step is to run the Waterhole installation command. This will create the Waterhole database tables, seed some initial data, and prompt you to create your administrator account.

```bash
php artisan waterhole:install
```

## Next Steps

Navigate to your community URL in your favorite web browser. If everything's been hooked up correctly, you should see a fresh Waterhole installation, and be able to log in with your administrator account.

From here, there are a few things you might want to do:

-   Learn how Waterhole [Configuration](./configuration.md) works and configure a mail driver.
-   Customize your community's [Structure](./structure.md), [User Groups](./groups.md), [Design](./design.md), and Control Panel [Dashboard](./dashboard.md).
-   Learn the best practices for [Deploying](./deploying.md) your community to production.
-   Learn how to [extend](./extending.md) Waterhole with custom functionality.

<!-- - Learn how to install and manage [Extensions](extensions.md). -->
