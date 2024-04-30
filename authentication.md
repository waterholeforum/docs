# Authentication

Waterhole uses Laravel's Authentication system, and supports adding external authentication providers via Laravel Socialite.

## Passwords

By default, Waterhole supports authenticating with passwords. If you would like to only allow registering and logging in via an external auth provider, you can disable passwords by setting the `password_enabled` setting to `false` in `config/waterhole/auth.php`.

## OAuth

Waterhole supports OAuth authentication via [Laravel Socialite](https://github.com/laravel/socialite), which includes support for Facebook, Twitter, LinkedIn, Google, GitHub, GitLab, and Bitbucket.

The [Socialite Providers](https://socialiteproviders.com/) organization contains many more providers that you can use as well. And, if you require a provider not on the list, you can [create your own](https://medium.com/laravel-news/adding-auth-providers-to-laravel-socialite-ca0335929e42).

### Configuring OAuth

You can configure OAuth behavior in `config/waterhole/auth.php`. Add the OAuth providers you want to support to the `providers` array. Waterhole will show buttons for these providers on the login and registration pages:

```php
'providers' => [
    'google',
    'github',
],
```

Add your provider credentials to `config/services.php`, and set the `redirect` option to `/auth/{provider}/callback`:

```php
'github' => [
    'client_id' => env('GITHUB_CLIENT_ID'),
    'client_secret' => env('GITHUB_CLIENT_SECRET'),
    'redirect' => '/auth/github/callback',
],
```

### Button Customization

Waterhole includes styles for the default providers supported by Socialite, but if you wish to change them, or for any additional providers, you can customize the icon and name displayed on a provider's button by providing a configuration array:

```php
'providers' => [
    'google',
    'github' => [
        'icon' => 'tabler-brand-github',
        'name' => 'GitHub',
    ],
],
```

To style a provider button, target it using the `.auth-button[data-provider]` selector:

```css
.auth-button[data-provider='github'] {
    background: black;
    color: white;
}
```

## Single Sign-On (SSO)

If you have a site with an existing user base and authentication system that you would like to use, Waterhole includes a special authentication provider to help set this up.

When users try to log in using this provider, they will be redirected to your website along with a signed payload. You will need to configure your website to verify this payload, perform authentication, and then redirect the user back to your Waterhole site with the authenticated user details.

### Configuring Waterhole

To get started, add the `sso` provider to the `providers` array in `config/waterhole/auth.php`. Usually you will want this to be the only provider, and will also want to disable Waterhole's built-in password authentication:

```php
'password_enabled' => false,

'providers' => [
    'sso',
],
```

You will also need to configure the `sso` settings in this file:

-   `url` is your website's URL where Waterhole will send users when they attempt to log in. For example, this might be `https://example.com/sso`.
-   `secret` is a secret string that will be used to sign payloads and ensure they are authentic. You should set this to a random string of 10 or more characters.

```php
'sso' => [
    'url' => env('WATERHOLE_SSO_URL'),
    'secret' => env('WATERHOLE_SSO_SECRET'),
],
```

### Configuring Your Site

You will need to configure your existing website to respond to requests at the URL you configured in the previous step. Waterhole will send users to this URL along with two query parameters: `payload` and `sig`. Your website needs to validate these query parameters, perform authentication, and then construct a URL to return the user back to Waterhole.

> **Danger:** Waterhole uses emails to map external users to Waterhole users, and assumes that external emails are verified. This means that **you must validate emails** on your website before sending them to Waterhole, otherwise your forum will be extremely vulnerable.

#### PHP

If your website uses PHP, you can use the `waterhole/sso` library to handle SSO requests for you. First, install it using Composer:

```bash
composer require waterhole/sso
```

Then, in your controller, once the user has successfully authenticated and verified their email address, instantiate the `WaterholeSso` class with your SSO `secret`, and call the `authenticate` method with an instance of [`Waterhole\Sso\PendingUser`](reference://Waterhole/Sso/PendingUser.html) (see below to learn about the available parameters). In Laravel this might look like:

```php
use Waterhole\Sso\PendingUser;
use Waterhole\Sso\WaterholeSso;

Route::middleware('auth', 'verified')->get('sso', function () {
    $sso = new WaterholeSso('sso_secret');

    $user = Auth::user();

    $sso->authenticate(
        new PendingUser(
            identifier: $user->id,
            email: $user->email,
            name: $user->name,
            avatar: $user->avatar_url,
        )
    );
});
```

#### Other Languages

If your website does not use PHP, you will need to write code to perform the following steps. Refer to the PHP `waterhole/sso` library as a reference implementation.

1. **Validate the signature.** Ensure that HMAC-SHA256 of the `payload` (using your `secret` as the key) is equal to the `sig`.
2. **Parse the payload.** Base64 decode the payload and then parse it as a query string to extract the `nonce` and `returnUrl`.
3. **Construct the response payload.** Build a new query string with the `nonce`, `externalId`, `email`, `username`, and any other parameters you want to send back to Waterhole.
4. **Sign the response payload.** Base64 encode this payload then calculate a HMAC-SHA256 hash (using your `secret` as the key).
5. **Redirect back.** Send the user back to the `returnUrl` with the `payload` and `sig` query parameters.

#### `PendingUser` Parameters

-   `identifier` (required) is a unique ID for the user in your system that will never change.
-   `email` (required) must be a **verified** email address.
-   `name` is a suggested username for the user if they are new.
-   `avatar` is a URL that will be downloaded and set as the user's avatar if they are new.
-   `groups` is an array of group IDs that will be assigned to the user if they are new.
