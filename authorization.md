# Authorization

Waterhole has a powerful permissions system to allow granular control over what users can and can't do.

The authorization layer is made up of two parts:

- **A permissions table**, which allows the forum admin to define which user groups have access and abilities for each structure node (channels, pages, links).

- **[Laravel Gates](https://laravel.com/docs/9.x/authorization#gates)**, which enforce authorization on resources using the information in the permissions table, along with additional domain logic.

## The Permissions Table

The permissions database table defines permission entries which consist of the following:

- **Recipient** – the user or group that receives the permission
- **Ability** – the name of the ability (eg. `view` or `post`)
- **Scope** – the resource that the recipient has this ability for (usually a structure node, like a channel or page)

Permission data can be inspected using the `Waterhole\Models\PermissionCollection` singleton. This class provides [various methods](https://waterhole.dev/docs/reference/Waterhole/Models/PermissionCollection.html) to filter and check for the existence of permission entries:

```php
use Waterhole\Models\PermissionCollection;

$permissions = app(PermissionCollection::class);

// Check if permission has been granted for $user to post
// in $channel (via their user groups)
$permissions->user($user)->scope($channel)->allows('post');

// Shorter syntax for above
$permissions->can($user, 'post', $channel);

// Get the IDs of the channels which are visible to guests
$permissions->guest()->scope(Channel::class)->ability('view')->ids();
```

> **Warning:**  
> You should not call the `PermissionCollection` class directly in your controller or action code! Instead, you should define a [Gate](#extending-authorization-logic) for the ability, and then use the `PermissionCollection` class inside the gate logic. This allows you to benefit from the ergonomics of the Gate (like calling `authorize` to automatically throw HTTP errors), and also allows authorization logic to be extended.

## Checking Abilities With Gates

While the permissions table merely stores information about permissions, [Laravel Gates](https://laravel.com/docs/9.x/authorization#gates) are used to actually enforce authorization. Laravel Policies are not used because they are not extensible.

A Gate is defined for each ability. The permissions table is generally used to determine whether or not the user is allowed to do something, but Gates can also apply other domain logic. For example: a user can edit a post if it is their own, or if they have the "moderate" permission for the post's channel.

Additionally, some logic is applied globally across all Gate checks:

- If the user is an administrator, permission is granted
- If the user has not yet verified their email, they have the same permissions as a guest

Gate abilities are named using the format `resource.ability`. For example, the Gate to check whether a post can be edited is named `post.edit`.

Use Gate methods, as per the [Laravel Gate documentation](https://laravel.com/docs/9.x/authorization#authorizing-actions-via-gates), to authorize actions. For example, to assert that the current user can edit a post:

```php
Gate::authorize('post.edit', $post);
```

## Extending Authorization Logic

To extend the logic for an existing ability, use the `Gate::before` method to conditionally intercept Gate checks:

```php
Gate::before(function (User $user, $ability, $arguments) {
    if ($ability === 'post.edit') {
        $post = $arguments[0];
        // return true if permission is granted
        // return false if permission is denied
        // return null to defer to the existing logic
    }
});
```
