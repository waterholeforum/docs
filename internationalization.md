# Internationalization

Waterhole features a powerful localization system using
[Project Fluent](https://projectfluent.org) on top of the Laravel Translator.

If you're developing an [extension](./distribution.md), or if your forum is
multi-lingual, you should take advantage of Waterhole's internationalization
capabilities.

Fluent is a fully-featured localization system for natural-sounding
translations. Read the
[Fluent Syntax Guide](https://projectfluent.org/fluent/guide/) to learn more
about syntax and its benefits. See also
[Good Practices for Developers](https://github.com/projectfluent/fluent/wiki/Good-Practices-for-Developers).

Waterhole uses the
[`laravel-fluent` package](https://github.com/jrmajor/laravel-fluent) to replace
the default Laravel translator with one that supports loading translations from
`.ftl` files.

## Defining Translations

Translations are stored in files within the `resources/lang` directory. Within
this directory, create a subdirectory for each language you want to support. You
can add translations in both `.php` files in the Laravel
[short key format](https://laravel.com/docs/10.x/localization#using-short-keys),
and in Fluent `.ftl` files, with the latter taking precedence.

```
/resources
  /lang
    /en
      stream.ftl
      messages.php
    /fr
      stream.ftl
      messages.php
```

If you're developing an extension, you'll need to register your translation
files with a namespace in the `boot` method of a service provider:

```php
$this->loadTranslationsFrom(__DIR__.'/../resources/lang', 'acme-example');
```

## Retrieving Translations

You may retrieve translation strings from your language files using the `__`
helper function:

```php
__('stream.shared-photos', [
    'userName' => 'Toby',
    'photoCount' => 2,
    'userGender' => 'male',
]); // Toby added 2 new photos to his stream.
```

Extensions must use their registered namespace prefix:

```php
echo __('acme-example::messages.hello');
```

There is no need to use the `trans_choice` helper as the Fluent format
eliminates the need for another function.
