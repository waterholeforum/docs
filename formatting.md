# Formatting

Waterhole uses the [TextFormatter](https://github.com/s9e/TextFormatter) library
to safely format markup in posts and comments. You can hook in to add or remove
formatting syntax and change rendered HTML.

Before getting started, it is worth reading the TextFormatter documentation to
understand
[how it works](https://s9etextformatter.readthedocs.io/Getting_started/How_it_works/)
at a high level, and what plugins and settings are available. The
[TextFormatter community](https://github.com/s9e/TextFormatter/discussions) is a
great place to get support.

## Configuration

By default, Waterhole
[configures the formatter](https://github.com/waterholeforum/core/blob/main/src/Providers/FormatterServiceProvider.php)
to parse Markdown syntax, @mentions, and auto-link URLs and emails.

To apply your own configuration to the formatter, pass a callback to the
`Formatter` extender's `configure` method:

```php
use s9e\TextFormatter\Configurator;
use Waterhole\Extend;

$this->extend(function (Extend\Core\Formatter $formatter) {
    $formatter->configure(function (Configurator $configurator) {
        $configurator->BBCodes->addFromRepository('B');
    });
});
```

You will need to clear the formatter cache in order for any changes to take
effect:

```bash
php artisan waterhole:cache:clear
```

## Parsing

In the parsing phase, user-submitted text is parsed into an XML document for
storage in the database. To hook into this phase of formatting, pass a callback
to the `Formatter` extender's `parsing` method:

```php
use s9e\TextFormatter\Parser;
use Waterhole\Extend;
use Waterhole\Formatter\Context;

$this->extend(function (Extend\Core\Formatter $formatter) {
    $formatter->parsing(function (Parser $parser, string &$text, ?Context $context) {
        $parser->disablePlugin('BBCodes');
    });
});
```

Here you can perform
[runtime configuration](https://s9etextformatter.readthedocs.io/Getting_started/Runtime_configuration/)
on the parser, or make manual alterations to the `$text` before it is parsed.

The `$context` parameter may provide information about the model and user for
which the text is being parsed.

## Rendering

The rendering phase is when the XML document stored in the database is
transformed into HTML. To hook into this phase of formatting, pass a callback to
the `Formatter` extender's `rendering` method:

```php
use s9e\TextFormatter\Renderer;
use Waterhole\Extend;
use Waterhole\Formatter\Context;

$this->extend(function (Extend\Core\Formatter $formatter) {
    $formatter->rendering(function (Renderer $renderer, string &$xml, ?Context $context) {
        $renderer->setParameter('USER', 'Joe');
    });
});
```

Here you can perform
[runtime configuration](https://s9etextformatter.readthedocs.io/Getting_started/Runtime_configuration/)
on the renderer, or make manual alterations to the `$xml` document before it is
rendered.

The `$context` parameter may provide information about the model and user for
which the text is being rendered.
