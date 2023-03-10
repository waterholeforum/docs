# Customizing The Design

Waterhole features a simple and adaptable design that's easy to customize and integrate with your brand.

## Logo

Place your brand's logo under a `public/images` directory, and then set `logo_url`:

```php
    'logo_url' => asset('images/logo.png'),
```

Your logo will show up in the forum header and in your forum's email template. For full support in email clients, you must use a JPG or PNG file.

If you'd like to show an SVG version of your logo in the forum header, you can [override](https://laravel.com/docs/9.x/packages#overriding-package-views) the "header title" template by creating a file at `resources/views/vendor/waterhole/components/header-title.blade.php`:

```blade
<a href="{{ route('waterhole.home') }}">
    <!-- Your logo SVG here -->
</a>
```

## Custom CSS

Waterhole's [design system](./design/overview.md) makes heavy use of CSS variables and classes, both of which you can override and extend.

New Waterhole projects are already hooked up with a file where you can place CSS customizations: `resources/css/waterhole/app.css`.

Use your browser's dev tools to inspect elements and their classes and styles to help target your customizations.

### Dark Mode

Waterhole features a user Dark Mode toggle button in the header. To target changes to CSS variables and classes in dark mode, put them within a `:root[data-theme="dark"]` selector:

```css
:root[data-theme="dark"] {
  /* dark mode RULES! */
}
```

If you don't want to support Dark Mode on your forum, you can disable the toggle completely by setting `support_dark_mode` to `false` in `config/waterhole/design.php`.

## Custom HTML

Waterhole's HTML is server-rended using [Blade](https://laravel.com/docs/9.x/blade) views and components. Many parts of Waterhole's templates render **dynamic lists of components**. For example, the page header is made up of these components:

- The forum title
- A "spacer" element
- The search button
- The notifications button
- The user menu
- The theme selector

Using [extenders](./extending.md#extenders), you can hook into these component lists and add your own components at any position, or remove existing ones. Some common examples are listed below. To learn more about how component lists work, see the [Frontend](./frontend.md#component-lists) page.

### `<head>`

To add custom HTML inside the `<head>` tag, add the following to the `boot` method of `app/Providers/WaterholeServiceProvider.php`:

```php
Extend\DocumentHead::add('waterhole.head');
```

The argument is the name of a view where your custom template resides – in this example `waterhole.head` would correspond to a template which you should create at `resources/views/waterhole/head.blade.php`.

### Site Header

To add a custom site header above Waterhole's header, add the following to the `boot` method of `app/Providers/WaterholeServiceProvider.php`:

```php
Extend\LayoutBefore::add('partials.header', position: -10);
```

Here we're referring to a template which you should create at `resources/views/partials/header.blade.php`. We also pass the `position` parameter to render our custom header **before** Waterhole's header.

### Hero

You can add a "hero" to your forum's index page to welcome users to your community. Add the following to the `boot` method of `app/Providers/WaterholeServiceProvider.php`:

```php
Extend\LayoutBefore::add('waterhole.hero');
```

Your `resources/views/waterhole/hero.blade.php` template might look something like this. The `@if` conditional will make sure it only renders on the index page:

```blade
@if (Request::routeIs('waterhole.home', 'waterhole.channels.show'))
    <section class="section container row justify-center text-center">
        <p class="measure lead">
            Welcome to my community! Please take a look around.
        </p>
    </section>
@endif
```

### Footer

Since some of Waterhole's interfaces make use of infinite scrolling, placing footer content at the very bottom of the page is not a good idea. Instead, it is recommended to append content to the bottom of the sidebar on the index page. Add the following to the `boot` method of `app/Providers/WaterholeServiceProvider.php`:

```php
Extend\IndexFooter::add('waterhole.footer');
```

### Other Extenders

Take a look at all of the extenders that are available under the [`Waterhole\Extend` namespace](https://waterhole.dev/docs/reference/Waterhole/Extend.html).
