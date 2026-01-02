# Filters

Waterhole allows customization of the filters on post and comment feeds.

A filter is a set of filtering or sorting criteria that can be applied to a query, like "Latest" or "Top".

## Configuring Filters

For the post feed on the forum index, see the `post_filters` option in `config/waterhole/forum.php`. Filters can also be [configured for each channel](./structure.md#channel-options).

For the post and comment feeds on the user profile page, see the `post_filters` and `comment_filters` options in `config/waterhole/users.php`.

## Defining Filters

To define a new filter, extend the `Waterhole\Filters\Filter` class:

```php
namespace App\Filters;

use Illuminate\Database\Eloquent\Builder;
use Waterhole\Filters\Filter;

class Unanswered extends Filter
{
    /**
     * The text label for the filter.
     */
    public function label(): string
    {
        return 'Unanswered';
    }

    /**
     * Apply the filter to the feed query builder.
     */
    public function apply(Builder $query): void
    {
        $query->where('is_answered', false);
    }
}
```

## Registering Post Filters

Post filters need to be registered in order to make them available for selection when configuring a channel. To register a post filter, call the `add` method on the `PostFilters` extender:

```php
use App\Filters;
use Waterhole\Extend;

$this->extend(function (Extend\Core\PostFilters $filters) {
    $filters->add(Filters\Unanswered::class);
});
```
