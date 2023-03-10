# Assets

Learn how to add scripts, stylesheets, and other public assets to the Waterhole layout.

## Asset Bundles

Waterhole creates bundles of scripts and stylesheets to reduce the number of HTTP requests needed when loading the page. You can register your own files to be included in these bundles.

### Stylesheets

Add stylesheets to the bundle using the [`Stylesheet`](https://waterhole.dev/docs/reference/Waterhole/Extend/Stylesheet.html) extender. Waterhole will concatenate stylesheets onto the end of the bundle:

```php
use Waterhole\Extend;

Extend\Stylesheet::add('styles.css');
```

### Scripts

Add scripts to the bundle using the [`Script`](https://waterhole.dev/docs/reference/Waterhole/Extend/Concerns/Script.html) extender. Waterhole will concatenate scripts onto the end of the bundle:

```php
Extend\Script::add('script.js');
```

### Admin Bundle

By default, assets are added to the main bundle which is loaded on every page – both in the forum and admin section. If you want to add assets to be loaded only in the admin section, specify the bundle name as `admin`:

```php
Extend\Stylesheet::add('styles.css', bundle: 'admin');
```

## Public Assets

If you have public assets like images, or scripts and stylesheets that you don't want to include in the bundle, simply place them in your application's `public` directory and then refer to them using Laravel's `asset()` function.

If you're developing an [extension](./distribution.md), use the service provider's `publishes` method to publish your assets to the application's `public` directory:

```php
$this->publishes(
    [__DIR__.'/../public' => public_path('vendor/my-extension')],
    'public'
);
```

## External Assets

To link to an external asset (eg. Google Fonts or something else from a CDN), don't use the `Stylesheet` or `Script` extenders. Instead, use the [`DocumentHead` extender](https://waterhole.dev/docs/reference/Waterhole/Extend/DocumentHead.html), which is an [ordered list](https://waterhole.dev/docs/reference/Waterhole/Extend/Concerns/OrderedList.html) of views to output in the `<head>` tag.

```php
use Waterhole\Extend;

Extend\DocumentHead::add('partials.font');
```
