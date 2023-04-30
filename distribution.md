# Extension Distribution

You can reuse and distribute your customizations and features as extensions – packages that can be installed with Composer.

> **Tip:** If you're building customizations specific to your community, then you probably don't need to make an extension. Instead, you can put your customizations in your application-level service providers (`app/Providers`).

## Creating an Extension

Scaffold out a new Waterhole extension by running the following command:

```
php artisan waterhole:make:extension acme/example
```

Your extension will be created inside the `extensions` directory. It will have a `composer.json` file and a service provider. Waterhole will automatically configure a Composer [path repository](https://getcomposer.org/doc/05-repositories.md#path) for your new extension, and install it as a dependency.

### Waterhole Metadata

Your extension's `composer.json` is pretty standard – it contains a PSR-4 autoloading rule, and tells Laravel about your service provider. But it also contains some Waterhole-specific metadata in the `extra` section which you'll want to fill out:

```json
{
    "extra": {
        "waterhole": {
            "name": "Example"
        }
    }
}
```

### Version Compatibility

You should specify which version(s) of Waterhole your extension is compatible with by including it in the `require` section of your extension's `composer.json` file:

```json
{
    "require": {
        "waterhole/core": "^0.1"
    }
}
```

Refer to the Composer documentation to learn more about [writing version constraints](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints).

## Publishing an Extension

Once your extension is ready for the world, you can [publish it on Packagist](https://packagist.org). Other Waterhole developers will be able to install your extension by running:

```
composer require acme/example
```

An official Waterhole Marketplace is planned, where you'll be able to list and sell your extensions.
