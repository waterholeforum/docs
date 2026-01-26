# Migrating from Other Platforms

If you have an existing community running on another platform, you may be able
to migrate it to Waterhole.

Currently Waterhole provides experimental support for migrating data from
[Flarum](https://flarum.org). Support for other platforms is planned – please
[get in touch](https://waterhole.dev/support) if you would like to work with us
to develop migration support for another platform.

## Migration Steps

### 1. Start With a Fresh Installation

You can only import data from another platform into a **fresh Waterhole
installation, before the `waterhole:install` command has been run**. The
Waterhole database must be completely empty to prevent any primary key conflicts
with imported data.

Follow the [installation](./installation.md) process, but instead of running the
`waterhole:install` command, run the database migrations directly:

```bash
php artisan migrate
```

This will set up the database without seeding any data or user accounts.

### 2. Install the Import Package

Run the following command to install the Waterhole Import package:

```bash
composer require waterhole/import
```

This package contains commands which we will use to import data from the legacy
platform.

### 3. Add a Database Connection

Open up `config/database.php` and add a new database connection for your legacy
platform's database. The easiest way to do this is to copy and paste one of the
existing blocks in the `connections` array (usually the `mysql` one), and change
its name and details. For example:

```php
'connections' => [
    // ...

    'flarum' => [
        'driver' => 'mysql',
        'host' => '127.0.0.1',
        'port' => '3306',
        'database' => 'flarum',
        'username' => 'flarum',
        'password' => 'password',
        'unix_socket' => '',
        'charset' => 'utf8mb4',
        'collation' => 'utf8mb4_unicode_ci',
        'prefix' => '',
        'prefix_indexes' => true,
        'strict' => true,
        'engine' => null,
        'options' => extension_loaded('pdo_mysql') ? array_filter([
            PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
        ]) : [],
    ],
]
```

### 4. Run the Import Command

Run the appropriate `waterhole:import:*` command to import data from the legacy
platform. You will be asked which database connection to import data from –
select the connection that you configured in the previous step. The import will
be attempted and any progress and errors will be displayed.

## Supported Platforms

### [Flarum](https://flarum.org)

```bash
php artisan waterhole:import:flarum
```

- Users and groups are imported.
- Primary tags are imported as channels. All other tags are not.
- "Discussions" are imported as posts, and "comment posts" are imported as
  comments, including likes. Private or deleted discussions and posts are not.
