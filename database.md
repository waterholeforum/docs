# Database

Waterhole uses standard Laravel database migrations and Eloquent models.

## Migrations

As with any standard Laravel application, you can generate and run
[database migrations](https://laravel.com/docs/10.x/migrations) as needed.

If you're developing an extension, don't forget to
[load your migrations](https://laravel.com/docs/10.x/packages#migrations) in
your service provider.

## Models

Waterhole's [Eloquent](https://laravel.com/docs/10.x/eloquent) models are found
in the [`Waterhole\Models` namespace](reference://Waterhole/Models.html).

### Defining Relationships

To define new relations on Waterhole's models, use the
[`resolveRelationUsing` method](https://laravel.com/docs/10.x/eloquent-relationships#dynamic-relationships),
typically in the boot method of a service provider:

```php
use App\Models\Address;
use Waterhole\Models\User;

User::resolveRelationUsing('address', function (User $user) {
    return $user->belongsTo(Address::class, 'address_id');
});
```

### Model Events

You can listen for the
[standard events](https://laravel.com/docs/10.x/eloquent#events) dispatched by
Eloquent models to hook into the following moments in a model's lifecycle:
`retrieved`, `creating`, `created`, `updating`, `updated`, `saving`, `saved`,
`deleting`, `deleted`, `restoring`, `restored`, and `replicating`.

```php
use Waterhole\Models\User;

User::created(function (User $user) {
    // ...
});
```

Waterhole models also dispatch an `initialized` event whenever a new model
instance is constructed. This can be useful if you need to define new
[attribute casts](https://laravel.com/docs/10.x/eloquent-mutators#attribute-casting),
or set the default value of an attribute:

```php
use App\Models\Address;
use Waterhole\Models\User;

User::initialized(function (User $user) {
    $user->mergeCasts(['subscribed_at' => 'datetime']);
});
```
