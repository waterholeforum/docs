# Extending Waterhole

Waterhole is highly extensible, making it possible to add new features tailored to your community.

Waterhole is a Laravel package, meaning right off the bat you can add your own routes, views, middleware, container bindings, database migrations, console commands, and more, in the [standard Laravel way](https://laravel.com/docs/10.x).

But there are also plenty of opportunities to extend Waterhole itself. This section of the documentation covers those.

## Service Providers

In Laravel, all of the application bootstrapping – the registration of bindings, event listeners, middleware, routes – takes place in [service providers](https://laravel.com/docs/10.x/providers). This is also the place where you can put code to extend Waterhole.

If you're building customizations specific to your community, then you can add code to your project's service providers, found in `app/Providers`.

If, on the other hand, you want to reuse, distribute, or sell your features, you can [make an extension](./distribution.md). The premise is the same though – the starting point is your extension's service providers.

## Extenders

The main mechanism by which you'll hook into Waterhole is with **extenders**. These are static classes that provide methods to extend Waterhole in some way. You can see all of the extenders that are available under the [`Waterhole\Extend` namespace](reference://Waterhole/Extend.html).

As a quick example, open up `app/Providers/WaterholeServiceProvider.php` and add the following code to the `register` method:

```php
use Illuminate\View\Component;
use Waterhole\Extend;

public function register()
{
    Extend\LayoutBefore::add(
        new class extends Component {
            public function render()
            {
                return 'Hello, world!';
            }
        }
    );
}
```

Now reload your forum to see your first customization!

What did we just do? We used the `LayoutBefore` extender to inject a custom component into the Waterhole layout. There are dozens more extenders like this covering all parts of Waterhole's views and functionality, ready for you to hook into.

> **Warning:** To avoid strange behavior when using [Laravel Octane](https://laravel.com/docs/10.x/octane), extenders should always be called in the `register` method of a service provider, and should never be guarded by a request-specific condition. For example, **don't** do this:
>
> ```php
> if (Auth::check()) {
>     Extend\Header::add('foo', 'bar');
> }
> ```
>
> Instead, pass a closure to the extender and put the condition inside:
>
> ```php
> Extend\Header::add('foo', fn() => Auth::check() ? 'bar' : null);
> ```

## Next Steps

Using extenders in service providers, you can achieve a whole range of things within Waterhole:

-   Add [Actions](./actions.md) to the context menus of posts, comments, and other objects.
-   Add pages and widgets to the [Control Panel](./cp.md).
-   Inject frontend [Assets](./assets.md), like scripts and stylesheets.
-   Hook into Waterhole's [frontend](./frontend.md).
-   Run [Database](./database.md) migrations and queries.
-   Add custom ways to sort the post and comment lists using [Filters](./filters.md).
-   Add new text [Formatting](./formatting.md) syntax.
-   Make your features [localizable](./internationalization.md).
-   Add [Authentication](./authentication.md) providers.
-   Create new types of [Notifications](./notifications.md) to send to users.
-   Add your own [Routes](./routes.md) to the application.
