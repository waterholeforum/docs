# Localization

Waterhole can be translated into any language, and we welcome contributions from
the community.

Waterhole uses [Project Fluent](https://projectfluent.org), a fully-featured
localization system for natural-sounding translations. Read the
[Fluent Syntax Guide](https://projectfluent.org/fluent/guide/) to learn more
about syntax and its benefits.

## Configuration

Set the default language for your forum in `config/app.php`. You can also choose
a fallback locale in case any translations are lagging behind when new content
and strings are added.

```php
'locale' => 'fr',
'fallback_locale' => 'en',
```

### Available Languages

| Language | Code |
| -------- | ---- |
| English  | `en` |

Translations are community-contributed so they may not always be up-to-date. If
you find a missing or incorrect translation, you can
[contribute a fix](#contributing-new-translations).

## Local Overrides

You can override individual translation strings in your project by placing files
in your `resources/lang/vendor/waterhole/{locale}` directory. You can take a
look at
[Waterhole's language files](https://github.com/waterholeforum/core/tree/main/lang)
to find the translations you want to override.

If, for example, you'd like to override the English translation strings in
`forum.ftl`, then create a new file in your project called
`resources/lang/vendor/waterhole/en/forum.ftl`. Within this file, you should
only define the translation strings you wish to override. Any translation
strings you don't override will still be loaded from the Waterhole's original
language files.

```fluent
post-title-label = Subject
```

Refer to the [Fluent Syntax Guide](https://projectfluent.org/fluent/guide/) to
learn about the syntax for `.ftl` files.

## Contributing New Translations

To contribute new or missing translations to Waterhole, follow these steps:

1. Clone the `waterholeforum/core` Git repository locally.
2. Make changes to the translations in `resources/lang`:
    - To create translations for a new language, copy the files in the `en`
      directory into a new directory. You can name this directory a short 2
      character language code (`fr`) or the full 4 character regional code
      (`fr_CA`).
    - Edit the translation files. Refer to the
      [Fluent Syntax Guide](https://projectfluent.org/fluent/guide/) to learn
      about the syntax for `.ftl` files.
3. Commit your changes and submit a PR.
