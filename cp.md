# Control Panel

You can add your own pages and dashboard widgets to Waterhole's Control Panel.

## Pages

### Adding Routes

The Control Panel is accessible under `/cp` by default (configurable in `config/waterhole/cp.php`), and all routes are named with the `waterhole.cp.` prefix. To register your own routes with these attributes, use the `cp` method of the `Routes` extender:

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

Extend\CpRoutes::add(function () {
    Route::get('my-route', MyController::class) // Path: /cp/my-route
        ->name('my-route');                     // Name: waterhole.cp.my-route
});
```

> See [Routes](./routes.md) for more information.

### Adding a Navigation Link

The [`CpNav`](reference://Waterhole/Extend/CpNav.html) extender is a list of components that make up the CP navigation. You can add any component or view you want; typically you'll want to use a closure returning a [`NavLink`](reference://Waterhole/View/Components/NavLink.html) component instance:

```php
use Waterhole\Components\NavLink;
use Waterhole\Extend;

Extend\CpNav::add(
    'resources',
    fn() => new NavLink(
        label: 'Resources',
        icon: 'tabler-star',
        route: 'waterhole.cp.resources',
    )
);
```

### Rendering the CP Layout

Use the [`<x-waterhole::cp>`](reference://Waterhole/View/Components/Cp.html) component to render your views inside the CP layout:

```blade
<x-waterhole::cp title="My Page">
    <!-- content -->
</x-waterhole::cp>
```

### Adding Assets

To add styles or scripts to the Control Panel (without adding them to the global bundle), use the [`Stylesheet`](reference://Waterhole/Extend/Stylesheet.html) and [`Script`](reference://Waterhole/Extend/Script.html) extenders and specify `cp` as the bundle name.

```php
use Waterhole\Extend;

Extend\Stylesheet::add(__DIR__.'/../resources/less/test.css', bundle: 'cp');
Extend\Script::add(__DIR__.'/../resources/dist/test.js', bundle: 'cp');
```

> See [Assets](./assets.md) for more information about how Waterhole manages assets.

## Widgets

Extensions can make widgets available to be displayed on the CP [Dashboard](./dashboard.md).

### Defining a Widget

Widgets are just [Blade components](https://laravel.com/docs/10.x/blade#components). So, to make a new widget available, define a new component:

```php
namespace App\Widgets;

use Illuminate\View\Component;

class LatestResources extends Component
{
    public function render()
    {
        return view('widgets.latest-resources');
    }
}
```

Typically the widget view will contain a [card](./design/cards.md) beginning with a `<h3>` element:

```html
<div class="card">
    <h3>Latest Resources</h3>
    <!-- ... -->
</div>
```

### Widget Configuration

Each Waterhole project can configure its widget layout in `config/waterhole/cp.php` – refer to the [Dashboard](./dashboard.md) customization documentation.

Any extra configuration options will be passed in when the widget is instantiated. For example, you could accept a `limit` configuration option (with a default value) like so:

```php
class LatestResources extends Component
{
    public function __construct(public int $limit = 3)
    {
    }

    // ...
}
```

And then consumers can override it:

```php
'widgets' => [
    [
        'component' => App\Widgets\LatestResources::class,
        'width' => 1 / 2,
        'limit' => 5,
    ],
]
```

### Lazy Loading

If your widget is a bit resource intensive – if it makes a HTTP request, or runs an expensive database query, for example – you can indicate that it should be loaded lazily, so as to not slow down the whole Dashboard loading.

Set a static `$lazy` property to true on your component, and Waterhole will take care of the rest:

```php
class LatestResources extends Component
{
    public static bool $lazy = true;
}
```
