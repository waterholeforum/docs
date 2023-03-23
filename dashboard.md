# Admin Dashboard

Customize the widgets displayed on the Admin Dashboard.

The Dashboard page of the Admin Panel is a canvas on which you can lay out **widgets**. A widget might display useful information and insights about your community, or offer shortcuts or functionality. There are a few widgets available out of the box, and developers can also easily [build custom widgets](./admin.md#widgets) and distribute them as extensions.

On a fresh Waterhole installation, the dashboard is configured to show a few simple widgets: a Getting Started widget, a feed of the Waterhole blog, and line chart widgets showing the number of new users, posts, and comments in your community.

## Configuring Widgets

To customize the widgets on the dashboard, open up the `config/waterhole/admin.php` file and find the `widgets` setting. This is an array where each item represents a widget instance and its configuration:

```php
'widgets' => [
    // ...
    [
        'component' => Waterhole\Widgets\LineChart::class,
        'width' => 100 / 3,
        'title' => 'waterhole::admin.dashboard-users-title',
        'model' => Waterhole\Models\User::class,
    ],
    // ...
],
```

Each widget configuration array must have the following keys:

-   `component`: The name of the widget component class.
-   `width`: The minimum width of the widget on the dashboard. Numeric values represent a percentage – in the above example, `100 / 3` will make the widget take up a third of the available width. You can also specify a string containing a raw CSS `width` value (eg. `10em`). Note that widgets will grow to take up any extra space if available.

Any additional key-value pairs will be passed to the widget component as configuration options.

## Built-in Widgets

### Getting Started

```php
[
    'component' => Waterhole\Widgets\GettingStarted::class,
    'width' => 50,
],
```

The Getting Started widget displays a few helpful pointers on where to begin when setting up your Waterhole community. Once you have finished the steps, you may wish to remove this widget from your configuration.

### Feed

```php
[
    'component' => Waterhole\Widgets\Feed::class,
    'width' => 50,
    'url' => 'https://www.nasa.gov/rss/dyn/lg_image_of_the_day.rss',
    'limit' => 4,
],
```

The Feed widget displays the latest entries from an RSS or Atom feed, refreshing every 12 hours.

-   `url`: The URL of the RSS or Atom feed.
-   `limit`: The number of entries to display.

### Line Chart

```php
[
    'component' => Waterhole\Widgets\LineChart::class,
    'width' => 100 / 3,
    'title' => 'waterhole::admin.dashboard-users-title',
    'model' => Waterhole\Models\User::class,
],
```

The Line Chart widget displays a line chart showing the number of models created across a selectable time period, compared to the previous period.

-   `title`: A title for the widget. This can be a [translation key](./localization.md) or the text itself.
-   `model`: The class name of the model for which to display statistics.
-   `column`: The name of the timestamp column to track. Defaults to `created_at`.
-   `defaultPeriod`: The default period selected. Available periods are: `today`, `last_7_days`, `last_4_weeks`, `last_3_months`, `last_12_months`, `this_month`, `this_quarter`, `this_year`, `all_time`. Defaults to `last_7_days`.
