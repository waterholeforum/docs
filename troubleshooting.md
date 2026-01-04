# Troubleshooting

Uh oh, something's gone wrong! Here are some handy debugging tips, and some common problems and solutions.

## Debugging Tips

Whether you've encountered a White Screen of Death, a generic "internal server error" message, or some other broken behavior – sometimes it can be hard to work out what's gone wrong with your Waterhole installation. Here are a few things to try:

- **Check the logs.** These may contain more information about what's going wrong:

    - Waterhole logs in `storage/logs`
    - Web server logs (e.g. `/var/log/nginx/error.log`)
    - PHP-FPM logs (e.g. `/var/log/php8.x-fpm.log`)

- **Turn on debug mode.** Waterhole has a [debug mode](./configuration#debug-mode) which you can turn on to enable detailed error reporting in the browser. However, you should only use this for debugging on a development server – do NOT turn this on in production, as you risk exposing sensitive configuration values to the public.

- **Check the browser console.** If the problem is on the client-side rather than the server-side, look in your browser developer console for JavaScript errors. The Network tab can also be useful to expose server-side errors.

- **Clear your browser cache.** Sometimes, old versions of JavaScript and CSS assets can be cached by your browser. Try clearing your browser cache or doing a hard-refresh of the page to make sure you've got the latest assets.

- **Clear the application cache.** Likewise, sometimes old or corrupted data in the Waterhole/Laravel cache can cause outdated assets and translations to be served. Run `php artisan waterhole:cache:clear` to clear the Waterhole cache, and `php artisan cache:clear` to clear the Laravel cache.

* **Use the latest version of Waterhole.** Bugs and other issues can be fixed with newer versions of the software, so it's always a good idea to [keep up to date](./updating.md).

- **Check your configuration.** Go through your `.env` file and `config` directory, as well as the [configuration](./configuration.md) documentation, to make sure everything is correct.

- **Re-run optimizations.** If you're using certain [optimizations](./deploying.md#optimization) in your production environment, you need to re-run them on each deploy, otherwise old code will be cached. Any changes you make to the `.env` file will not take effect until the configuration cache is refreshed.

- **Disable local customizations.** Comment out any local customizations you've made in your app's `WaterholeServiceProvider` to see if you can isolate a problem.

- **Disable extensions.** Sometimes extensions can be the culprit. You can either remove them with the `composer remove` command or disable them temporarily by [opting out of package discovery](https://laravel.com/docs/10.x/packages#opting-out-of-package-discovery).

## Common Problems

### Assets Not Loading

If Waterhole's UI looks unstyled and the CSS/JS files aren't loading, try the following:

- Check for `public/storage`. If it doesn't exist, run `php artisan storage:link`.
- Confirm the built CSS/JS files are present inside `public/storage`.
- Verify `APP_URL` in `.env` matches the exact URL you're serving Waterhole from. If your forum is served at a different host (for example, `http://127.0.0.1:8000` vs `http://localhost`), the assets will 404.

## Get Help

Still no luck? Assuming you've now uncovered some more information about your problem (like an error message), it's time to do a bit of searching in the following places:

- [**The Waterhole community**](https://waterhole.dev/forum). Someone else might have had the same issue, posted about it, and received help in the community.

- [**The issue tracker**](https://github.com/waterholeforum/core/issues). You might have run into an issue that has already been reported, and maybe even fixed in an upcoming version.

If your search is proving unfruitful, [posting in the community](https://waterhole.dev/forum/posts/create?channel=3) is often the fastest way to get help. You can also [contact us](https://waterhole.dev/support) for support directly.

## Report a Bug

If you're confident that you've found an issue with Waterhole itself – not a problem on your end, or with an extension – please lodge it in our issue tracker by following the instructions on [Reporting Bugs](./contributing.md#bug-reports).
