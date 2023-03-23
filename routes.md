# Routes

Waterhole registers its routes with the [Laravel Router](https://laravel.com/docs/10.x/routing) in groups with a configurable prefix and custom middleware.

## Path Configuration

You can configure the path prefix for the main Waterhole routes in `config/waterhole/forum.php`, and for the admin panel in `config/waterhole/admin.php`.

## Registering Routes

To register your own routes in Waterhole's route groups, use the [`Routes` extender](https://waterhole.dev/docs/references/Waterhole/Extend/Routes.html). You can do this in the boot method of a service provider, or in your `routes/web.php` file.

For forum routes, use the `forum` method. Forum routes use the path prefix configured in `config/waterhole/forum.php`, a name prefix of `waterhole.`, and the `waterhole.web` middleware group.

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

Extend\Routes::forum(function () {
    Route::get('my-route', MyController::class)
        ->name('my-route'); // Name: waterhole.my-route
});
```

For [admin routes](./admin.md#adding-routes), use the `admin` method. Admin routes use the path prefix configured in `config/waterhole/admin.php` and a name prefix of `waterhole.admin.`. In addition to the `waterhole.web` middleware group, they use the `waterhole.admin` middleware group too, which ensures they are only accessible to admin users and require password confirmation.

## Adding Middleware

If you'd like to add your own middleware to be run on Waterhole requests, you call the `Route` facade's `prependMiddlewareToGroup` and `pushMiddlewareToGroup` methods:

```php
use Illuminate\Support\Facades\Route;

// Run on all requests
Route::pushMiddlewareToGroup('waterhole.web', MyMiddleware::class);

// Run on admin panel requests only
Route::pushMiddlewareToGroup('waterhole.admin', MyMiddleware::class);
```
