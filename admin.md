# Admin

You can add your own pages and dashboard widgets to Waterhole's admin section.

## Pages

### Adding Routes

The admin panel is accessible under `/admin` by default (configurable in `config/waterhole/admin.php`), and all routes are named with the `waterhole.admin.` prefix. To register your own routes with these attributes, use the `admin` method of the `Routes` extender:

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

Extend\Routes::admin(function () {
    Route::get('my-route', MyController::class) // Path: /admin/my-route
        ->name('my-route');                     // Name: waterhole.admin.my-route
});
```

> See [Routes](./routes.md) for more information.

### Adding a Navigation Link

The [`AdminNav`](https://waterhole.dev/docs/reference/Waterhole/Extend/AdminNav.html) extender represents an [ordered list](https://waterhole.dev/docs/reference/Waterhole/Extend/Concerns/OrderedList.html) of components that make up the admin navigation. You can add any component or view you want; typically you'll want to use a [`NavLink`](https://waterhole.dev/docs/reference/Waterhole/View/Components/NavLink.html) component instance:

```php
use Waterhole\Components\NavLink;
use Waterhole\Extend;

Extend\AdminNav::add(
    'resources',
    new NavLink(
        label: 'Resources',
        icon: 'tabler-star',
        route: 'waterhole.admin.resources',
    )
);
```

### Rendering the Admin Layout

Use the [`<x-waterhole::admin>`](https://waterhole.dev/docs/reference/Waterhole/View/Components/Admin.html) component to render your views inside the admin layout:

```blade
<x-waterhole::admin title="My Page">
    <!-- content -->
</x-waterhole::admin>
```

### Adding Assets

To add styles or scripts to the Administration section (without adding them to the global bundle), use the [`Stylesheet`](https://waterhole.dev/docs/reference/Waterhole/Extend/Stylesheet.html) and [`Script`](https://waterhole.dev/docs/reference/Waterhole/Extend/Script.html) extenders and specify `admin` as the bundle name.

```php
use Waterhole\Extend;

Extend\Stylesheet::add(__DIR__.'/../resources/less/test.less', bundle: 'admin');
Extend\Script::add(__DIR__.'/../resources/dist/test.js', bundle: 'admin');
```

> See [Assets](./assets.md) for more information about how Waterhole manages assets.

## Widgets

Extensions can make widgets available to be displayed on the admin [Dashboard](./dashboard.md).

### Defining a Widget

Widgets are just [Blade components](https://laravel.com/docs/10.x/blade#components). So, to make a new widget available, simply define a new component:

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

### Widget Assets

If your widget needs to include additional styles or scripts, you can call the `Styles` or `Scripts` extenders in your component constructor. This way, the resources will only be included if your widget is in use.

```php
use Waterhole\Extend;

class LatestResources extends Component
{
    public function __construct()
    {
        Extend\Stylesheet::add(__DIR__.'/../../resources/less/widget.less', bundle: 'admin');
        Extend\Script::add(__DIR__.'/../../resources/dist/widget.js', bundle: 'admin');
    }

    // ...
}
```

### Widget Configuration

Each Waterhole project can configure its widget layout in `config/waterhole/admin.php` – refer to the [Dashboard](./dashboard.md) customization documentation.

Any extra configuration options will be passed in when the widget is instantiated. For example, you could accept a `limit` configuration option (with a default value) like so:

```php
class LatestResources extends Component
{
    public int $limit;

    public function __construct(int $limit = 3)
    {
        $this->limit = $limit;
    }

    // ...
}
```

And then consumers can override it:

```php
'widgets' => [
    [
        'component' => App\Widgets\LatestResources::class,
        'width' => 50,
        'limit' => 5,
    ],
]
```

### Lazy Loading

If your widget is a bit resource intensive – if it makes a HTTP request, or runs an expensive database query, for example – you can indicate that it should be loaded lazily, so as to not slow down the whole Dashboard loading.

Simply set a static `$lazy` property to true on your component, and Waterhole will take care of the rest:

```php
class LatestResources extends Component
{
    public static bool $lazy = true;
}
```
