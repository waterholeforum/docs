# Configuration

Waterhole uses the standard Laravel configuration system – including config files and environment variables.

> Refer to the [Laravel Configuration](https://laravel.com/docs/10.x/configuration) documentation for more information.

## Config Files

Laravel configuration is stored in the `config` directory. These files contain framework-level settings, like database, mail, filesystem, and queue connection details.

Waterhole's configuration files are stored in the `config/waterhole` directory. These configuration files allow you to configure Waterhole-specific things, like dashboard widgets, and the number of comments per page.

All configuration options are documented, so feel free to browse through the files to see what options are available.

## Environment Variables

You'll often need to use different configuration settings based on the environment where the forum is running. For example, when developing locally, your site's URL might be `http://my-forum.test`, but in production it is `https://my-forum.com`.

You can store environment-specific configuration in the `.env` file, then refer to in the config files using the `env()` function. This allows you to commit the `config` directory to version control without including sensitive environment-specific information.

> **Tip:** Any variable in your `.env` file can be overridden by external environment variables such as server-level or system-level environment variables.

> **Warning:** Your `.env` file should never be committed to version control. Each developer/server will likely require a different environment configuration. Furthermore, this would be a security risk in the event an intruder gains access to your source control repository, since any sensitive credentials would get exposed.

### Environment Variable Types

Variables in your `.env` files are parsed as strings, with the exception of some reserved values to allow you to return a wider range of types:

| `.env` Value         | `env()` Value  |
| -------------------- | -------------- |
| `true` or `(true)`   | `(bool) true`  |
| `false` or `(false)` | `(bool) false` |
| `empty` or `(empty)` | `(string) ''`  |
| `null` or `(null)`   | `(null) null`  |

If you need to define an environment variable with a value that contains spaces, enclose the value in double quotes:

```php
APP_NAME="My Great Community"
```

### Retrieving Environment Variables

All environment variables are available in your config files by using the `env()` helper function. An optional second argument allows you to pass a default value.

```php
'debug' => env('APP_DEBUG', false),
```

> **Warning:** If you execute the `config:cache` command during your deployment process, you should be sure that you are only calling the `env` function from within your configuration files. Once the configuration has been cached, the `.env` file will not be loaded; therefore, the `env` function will only return external, system level environment variables.

## Mail Configuration

In order for Waterhole to be able to send out verification, password reset, and notification emails, you need to configure a mail driver. Mail driver configuration can be found in `config/mail.php`.

See the [Laravel Mail documentation](https://laravel.com/docs/10.x/mail#configuration) for more information about the available drivers.

To test your mail configuration, request a password reset on your account.

## Broadcasting Configuration

Waterhole can push out new activity over WebSockets so users will see updates in real-time without having to reload the page. To enable this, you'll need to configure a broadcaster in `config/broadcasting.php`.

See the [Laravel Broadcasting documentation](https://laravel.com/docs/10.x/broadcasting#pusher-channels) for more information about how to configure a driver. You will only need to install the driver on the server-side – Waterhole already contains the client-side `pusher-js` package and will hook everything up for you.

## Queue Configuration

You should strongly consider setting up a queue to process time-intensive tasks (like sending email notifications) in the background, so they don't slow down web requests. Queue configuration can be found in `config/queue.php`.

See the Laravel documentation for more information about the [available drivers](https://laravel.com/docs/10.x/queues#driver-prerequisites), how to [run the queue worker](https://laravel.com/docs/10.x/queues#running-the-queue-worker), and how to [configure Supervisor](https://laravel.com/docs/10.x/queues#supervisor-configuration) to ensure the queue works reliably in production. Or, you may wish to set up [Laravel Horizon](https://laravel.com/docs/10.x/horizon) to manage your queue workers.

## Cache Configuration

Waterhole makes use of a cache to boost performance. Cache configuration can be found in `config/cache.php`. By default, Waterhole is configured to use the `file` cache driver, which stores cached objects on the filesystem. In production, and especially on a larger community, you may wish to use a more robust driver such as Memcached or Redis.

See the [Laravel Cache documentation](https://laravel.com/docs/10.x/cache#configuration) for more information about the available drivers.

## Debug Mode

The `debug` option in your `config/app.php` configuration file determines how much information about an error is displayed to the user. When `true`, it also disables caching of asset bundles and translations.

By default, this option is set to respect the value of the `APP_DEBUG` environment variable, which is stored in your `.env` file. For local development, you should set the `APP_DEBUG` environment variable to `true`.

> **Danger:** In your production environment, `APP_DEBUG` should always be set to `false`. If the variable is set to `true` in production, you risk exposing sensitive configuration values to your forum's users. Additionally, your forum's performance will be reduced.

## Maintenance Mode

If you're performing some maintenance on your community and you want to temporarily disable access, you can use Laravel's [maintenance mode](https://laravel.com/docs/10.x/configuration#maintenance-mode).

To enable maintenance mode, run the `down` Artisan command:

```
php artisan down
```

And to disable maintenance mode, run the `up` Artisan command:

```
php artisan up
```
