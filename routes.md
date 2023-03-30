# Routes

Waterhole registers its routes with the [Laravel Router](https://laravel.com/docs/10.x/routing) in groups with a configurable prefix and custom middleware.

## Path Configuration

You can configure the path prefix for the main Waterhole routes in `config/waterhole/forum.php`, and for the Control Panel in `config/waterhole/cp.php`.

## Registering Routes

To register your own routes in Waterhole's route groups, use the [`ForumRoutes` extender](https://waterhole.dev/docs/references/Waterhole/Extend/ForumRoutes.html). You can do this in the boot method of a service provider.

Forum routes use the path prefix configured in `config/waterhole/forum.php`, a name prefix of `waterhole.`, and the `waterhole.web` middleware group.

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

Extend\ForumRoutes::add(function () {
    Route::get('my-route', MyController::class)
        ->name('my-route'); // Name: waterhole.my-route
});
```

For [Control Panel routes](./cp.md#adding-routes), use the `CpRoutes` extender. CP routes use the path prefix configured in `config/waterhole/cp.php` and a name prefix of `waterhole.cp.`. In addition to the `waterhole.web` middleware group, they use the `waterhole.cp` middleware group too, which ensures they are only accessible to admin users and require password confirmation.

## Adding Middleware

If you'd like to add your own middleware to be run on Waterhole requests, you call the `Route` facade's `prependMiddlewareToGroup` and `pushMiddlewareToGroup` methods:

```php
use Illuminate\Support\Facades\Route;

// Run on all requests
Route::pushMiddlewareToGroup('waterhole.web', MyMiddleware::class);

// Run on Control Panel requests only
Route::pushMiddlewareToGroup('waterhole.cp', MyMiddleware::class);
```
