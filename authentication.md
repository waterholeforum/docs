# Authentication
Waterhole uses Laravel's Authentication system, and supports adding OAuth authentication providers via Laravel Socialite.

## OAuth
Waterhole supports OAuth authentication via [Laravel Socialite](https://github.com/laravel/socialite), which includes support for Facebook, Twitter, LinkedIn, Google, GitHub, GitLab, and Bitbucket.

The [Socialite Providers](https://socialiteproviders.com/) organization contains many more providers that you can use as well. And, if you require a provider not on the list, you can [create your own](https://medium.com/laravel-news/adding-auth-providers-to-laravel-socialite-ca0335929e42).

### Configuring OAuth
Install Socialite with the following Composer command:

```
composer require laravel/socialite
```

You can configure OAuth behavior in `config/waterhole/auth.php`. Add the OAuth providers you want to support to the `oauth_providers` array. Waterhole will show buttons for these providers on the login and registration pages:

```php
'oauth_providers' => [
    'google',
    'github',
],
```

Add your provider credentials to `config/services.php`, and set the `redirect` option to `oauth/{provider}/callback`:

```php
'github' => [
    'client_id' => env('GITHUB_CLIENT_ID'),
    'client_secret' => env('GITHUB_CLIENT_SECRET'),
    'redirect' => 'oauth/github/callback',
],
```

### Button Customization

You can customize the icon and name displayed on a provider's button by providing a configuration array:

```php
'oauth_providers' => [
    'google',
    'github' => [
        'icon' => 'tabler-brand-github',
        'name' => 'GitHub',
    ],
],
```

To style a provider button, target it using the `.oauth-button[data-provider]` selector:

```css
.oauth-button[data-provider="github"] {
  background: black;
  color: white;
}
```
