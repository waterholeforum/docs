# Updating

Keeping your Waterhole installation up-to-date is easy, thanks to Composer.

> **Warning:** In production, you should always back up your database before updating.

To update Waterhole and any other dependencies to the latest release, run this Composer command:

```
composer update
```

And run any outstanding database migrations:

```
php artisan migrate
```

You should clear the Waterhole cache after updating to ensure that asset bundles and translations are refreshed:

```
php artisan waterhole:cache:clear
```

## Deployment Workflow

We strongly recommend setting up a [deployment workflow](./deploying.md) where all updates are installed and tested locally before they are pushed into production.

Before updating your production instance, you may wish to put your application into [maintenance mode](https://laravel.com/docs/10.x/configuration#pre-rendering-the-maintenance-mode-view) so that users do not encounter errors.
