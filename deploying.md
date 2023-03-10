# Deploying

Waterhole is a modern PHP application built on Laravel, and can be deployed like any standard Laravel application.

## Local Development

Running a copy of Waterhole on your computer is the preferred method for building and maintaining your community's features. This way you can keep your community's configuration and customization under version control, and easily test changes before you deploy them to production.

There are many ways to set up a local development server, from installing the stack from scratch, to using tools that do it for you like [Laravel Valet](https://laravel.com/docs/9.x/valet). You can also run a Docker development environment using [Laravel Sail](https://laravel.com/docs/9.x/sail).

## Production

The recommended way to deploy Waterhole in production is on a VPS, like those offered by [DigitalOcean](https://m.do.co/c/197189285163), [Linode](https://www.linode.com), and [Vultr](https://www.vultr.com). This gives you full control over the server and the ability to scale up resources on demand.

> **Warning:**  
> While shared hosting can be easier to manage, it is often not possible to gain SSH access and use Composer, both of which are requirements for running Waterhole. For this reason, **running Waterhole on shared hosting is not officially supported**.

> **Tip:**  
> Setting up and managing a VPS from scratch can be tedious. Consider using a service or tool that does this for you like [Laravel Forge](https://forge.laravel.com), [Ploi.io](https://ploi.io), [ServerPilot](https://serverpilot.io), or [Deployer](https://deployer.org)

Once your production server is configured, clone your source control repository, and run:

```
composer install
```

Then follow the rest of the [Installation](./installation.md) steps to establish your production Waterhole installation. Don't forget to [add your license key](./licensing.md) as well!

> **Danger:**  
> In your production environment, ensure that `APP_DEBUG` is set to `false` and `APP_ENV` is set to `production`. Otherwise, you risk exposing sensitive configuration values to your forum's users.

Whenever new changes are pushed into the repository, you can pull them in on your production server, then run:

```
composer install
php artisan migrate
```

This will install the latest dependencies and update the database.

With a service like [Laravel Forge](https://forge.laravel.com) or a tool like [Deployer](https://deployer.org) you can even automate this process so deployments will automatically be triggered when you push changes to a particular branch.

## Web Server Configuration

The main things that need to be configured on the web server are:

- The document root, which should point to the `public` directory
- URL rewriting to route all requests to the `public/index.php` file

> **Danger:**  
> Never attempt to move the `index.php` file to your project's root, as serving the application from the project root will expose many sensitive configuration files to the public Internet.

You should also ensure that static assets are served with cache headers and Gzip compression to optimize the performance of your community.

### Nginx

Waterhole includes a `.nginx.conf` file containing URL rewriting rules along with some additional best-practice performance configuration. You can manually copy this into your server block, or include it directly:

```
include /path/to/waterhole/.nginx.conf;
```

### Apache

Waterhole includes a `.htaccess` file containing URL rewriting rules along with some additional best-practice performance configuration. Make sure `mod_rewrite` is enabled and `AllowOverride All` is set.

## Optimization

Laravel offers various [optimization](https://laravel.com/docs/9.x/deployment#optimization) mechanisms which can speed up your Waterhole installation in production. You should run these in your deployment script:

```php
composer install --optimize-autoloader --no-dev
php artisan config:cache
php artisan route:cache
php artisan view:cache
```
