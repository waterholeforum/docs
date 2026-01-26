# Laravel Integration

Waterhole can be installed into an existing Laravel 10 or 11 application, and
you can set it up to use your existing Laravel user base too.

## Installation

### 1. Install via Composer

Add the `waterhole/core` package to your Laravel project by running the
following Composer command:

```bash
composer require waterhole/core --with-all-dependencies
```

Then, publish Waterhole's configuration files, and make sure the storage
directory is linked, by running the commands:

```bash
php artisan vendor:publish --tag=waterhole-config
php artisan storage:link
```

### 2. Configure the Database

To avoid Waterhole's database tables conflicting with your own application's,
you should set up a separate database connection for Waterhole – either using a
different database, or using a table prefix.

To do this, open up `config/database.php` and add a new database connection. The
easiest way to do this is to copy and paste the `mysql` block in the
`connections` array, and change its name and details. For example:

```php
'connections' => [
    // ...

    'waterhole' => [
        'driver' => 'mysql',
        'host' => '127.0.0.1',
        'port' => '3306',
        'database' => 'waterhole',
        'username' => 'waterhole',
        'password' => 'password',
        // ...
    ],
]
```

Once this is done, change the `database` setting in
`config/waterhole/system.php` to point to your new database connection:

```php
'database' => 'waterhole',
```

### 3. Configure Routing

By default, Waterhole is configured to serve up your forum at the `/` route.
This is something you'll probably want to change, assuming your app has existing
routes of its own.

To configure this, open up `config/waterhole/forum.php` and set the `path`
option. For example, if you want your forum to be accessible at
`https://example.com/forum`, set this to `forum`.

```php
'path' => 'forum',
```

You can also configure the path for the Waterhole Control Panel in
`config/waterhole/cp.php`, which is `cp` by default. And, you can configure
Waterhole's routes to be scoped to a subdomain by setting the `domain` option in
`config/waterhole/system.php`.

### 4. Run the Installer

Now that everything has been configured, the final step is to run the Waterhole
installation command. This will create the Waterhole database tables, seed some
initial data, and prompt you to create your administrator account.

```bash
php artisan waterhole:install
```

## Authentication

Waterhole provides a simple mechanism to use your Laravel app's existing user
base and authentication system.

To keep things clean, Waterhole still maintains its own `users` table separate
from your app's `users` table. After implementing an interface on your app's
`User` model, Waterhole will automatically detect when a user is logged in on
your app's default authentication guard. It will then load their linked
Waterhole user record, or create one if it doesn't already exist.

### 1. Disable Waterhole Authentication

First, we need to disable Waterhole's own authentication system. In the
Waterhole auth config file (`config/waterhole/auth.php`), set
`allow_regsistration` and `password_enabled` both to `false`, and make sure
there are no authentication providers in the `providers` array.

```php
'allow_registration' => false,
'password_enabled' => false,
```

Since all authentication methods have been disabled, Waterhole will no longer
register its login route. You can re-register it so that actions requiring
authentication will redirect guests to your app's own login page. Add the
following to your `app/Providers/WaterholeServiceProvider.php`:

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

public function register(): void
{
    $this->extend(function (Extend\Routing\ForumRoutes $routes) {
        Route::name('login')->get(
            'login',
            fn() => redirect()
                ->setIntendedUrl(url()->previous())
                ->route('login'),
        );
    });
}
```

### 2. Update Your User Model

Now implement the
[`Waterhole\Auth\AuthenticatesWaterhole`](reference://Waterhole/Auth/AuthenticatesWaterhole.html)
interface on your app's `User` model, and add the
[`Waterhole\Auth\HasWaterholeUser`](reference://Waterhole/Auth/HasWaterholeUser.html)
trait. This will tell Waterhole how to set up and link a corresponding Waterhole
user record when an authenticated user visits your forum.

```php
namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use Waterhole\Auth\AuthenticatesWaterhole;
use Waterhole\Auth\HasWaterholeUser;

class User extends Authenticable implements AuthenticatesWaterhole
{
    use HasWaterholeUser;
}
```

By default, Waterhole will carry across your user's `email` attribute, and use
their `name` attribute as a suggested username. If you wish to customize the
Waterhole user that gets created, you can override the `toWaterholeUser` method
which should return an instance of
[`Waterhole\Sso\PendingUser`](reference://Waterhole/Sso/PendingUser.html):

```php
public function toWaterholeUser(): ?PendingUser
{
    return new PendingUser(
        identifier: $this->getAuthIdentifier(),
        email: $this->email,
        name: $this->name,
        forceName: true,
    );
}
```

The `HasWaterholeUser` trait also registers a model listener so that whenever a
user's email is updated, the new email is automatically synced across to their
Waterhole user record.
