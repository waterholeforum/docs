# Routes

Waterhole registers its routes with the
[Laravel Router](https://laravel.com/docs/10.x/routing) in groups with a
configurable prefix and custom middleware.

## Path Configuration

You can configure the path prefix for the main Waterhole routes in
`config/waterhole/forum.php`, and for the Control Panel in
`config/waterhole/cp.php`.

## Registering Routes

To register your own routes in Waterhole's route groups, use the
[`ForumRoutes` extender](reference://Waterhole/Extend/Routing/ForumRoutes.html).
You can do this in a service provider by registering routes directly inside the
extender callback using the `Route` facade.

Forum routes use the path prefix configured in `config/waterhole/forum.php`, a
name prefix of `waterhole.`, and the `waterhole.web` middleware group.

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

$this->extend(function (Extend\Routing\ForumRoutes $routes) {
    Route::get('my-route', MyController::class)
        ->name('my-route'); // Name: waterhole.my-route
});
```

For [Control Panel routes](./cp.md#adding-routes), use the `CpRoutes` extender.
CP routes use the path prefix configured in `config/waterhole/cp.php` and a name
prefix of `waterhole.cp.`. In addition to the `waterhole.web` middleware group,
they use the `waterhole.cp` middleware group too, which ensures they are only
accessible to admin users and require password confirmation.

## Adding Middleware

If you'd like to add your own middleware to be run on Waterhole requests, you
call the `Route` facade's `prependMiddlewareToGroup` and `pushMiddlewareToGroup`
methods:

```php
use Illuminate\Support\Facades\Route;

// Run on all requests
Route::pushMiddlewareToGroup('waterhole.web', MyMiddleware::class);

// Run on Control Panel requests only
Route::pushMiddlewareToGroup('waterhole.cp', MyMiddleware::class);
```
