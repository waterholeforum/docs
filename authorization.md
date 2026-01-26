# Authorization

Waterhole has a powerful permissions system to allow granular control over what users can and can't do.

The authorization layer is made up of two parts:

- **A permissions table**, which allows the forum admin to define which user groups have access and abilities for each structure node (channels, pages, links).

- **[Laravel Gates](https://laravel.com/docs/10.x/authorization#gates)**, which enforce authorization on resources using the information in the permissions table, along with additional domain logic.

## The Permissions Table

The permissions database table defines permission entries which consist of the following:

- **Recipient** – the user or group that receives the permission
- **Ability** – the name of the ability (e.g. `view` or `post`)
- **Scope** – the resource that the recipient has this ability for (usually a structure node, like a channel or page)

Permission data can be inspected using the `Waterhole::permissions()` collection. This class provides [various methods](reference://Waterhole/Models/PermissionCollection.html) to filter and check for the existence of permission entries:

```php
use Waterhole\Waterhole;

// Check if permission has been granted for $user to post
// in $channel (via their user groups)
Waterhole::permissions()->can($user, 'post', $channel);

// Get the IDs of the channels which are visible to guests
Waterhole::permissions()->ids(null, 'view', Channel::class);
```

> **Warning:** Generally you should not check `Waterhole::permissions()` directly in your controller or action code! Instead, you should define a [Gate](#extending-authorization-logic) for the ability, and then check `Waterhole::permissions()` inside the gate logic. This allows you to benefit from the ergonomics of the Gate (like calling `authorize` to automatically throw HTTP errors), and also allows authorization logic to be extended.

## Checking Abilities With Gates

While the permissions table merely stores information about permissions, [Laravel Gates](https://laravel.com/docs/10.x/authorization#gates) are used to actually enforce authorization. (Laravel Policies are not used because they are not extensible.)

A Gate is defined for each ability. The permissions table is generally used to determine whether or not the user is allowed to do something, but Gates can also apply other domain logic. For example: a user can edit a post if it is their own, or if they have the "moderate" permission for the post's channel.

Additionally, some logic is applied globally across all Gate checks:

- If the user has not yet verified their email, or if they have been suspended, they are treated as if they are a guest
- If a Gate check returns `null` (i.e. no policy explicitly denies the ability), admins are allowed

Gate abilities are named using the format `waterhole.resource.ability`. For example, the Gate to check whether a post can be edited is named `waterhole.post.edit`.

Use Gate methods, as per the [Laravel Gate documentation](https://laravel.com/docs/10.x/authorization#authorizing-actions-via-gates), to authorize actions. For example, to assert that the current user can edit a post:

```php
Gate::authorize('waterhole.post.edit', $post);
```

If a Gate returns `null` and the ability name matches `waterhole.resource.ability`,
Waterhole will fall back to the permissions table for the ability and scope.

> Refer to Waterhole's [`AuthServiceProvider`](https://github.com/waterholeforum/core/blob/main/src/Providers/AuthServiceProvider.php) class to see the available abilities.

## Extending Authorization Logic

To extend the logic for an existing ability, use the `Gate::before` method to conditionally intercept Gate checks:

```php
Gate::before(function (User $user, $ability, $arguments) {
    if ($ability === 'waterhole.post.edit') {
        $post = $arguments[0];
        // return true if permission is granted
        // return false if permission is denied
        // return null to defer to the existing logic
    }
});
```
