# Icons

Various classes to help you work with icons.

## CSS

Use the `.icon` class to give an icon the correct size and alignment relative to surrounding text:

```html render
Text
<svg
    class="icon"
    fill="none"
    viewBox="0 0 24 24"
    stroke="currentColor"
    stroke-width="2"
    stroke-linecap="round"
    stroke-linejoin="round"
>
    <path d="M6 18L18 6M6 6l12 12" />
</svg>
Text
```

### Thickness

For line icons, the stroke thickness can be modulated using the `.icon--thin` and `.icon--thick` classes:

```blade render
@icon('tabler-star', ['class' => 'icon--thin text-xl'])
@icon('tabler-star', ['class' => 'icon--thick text-xl'])
```

### Width

While icon elements are always square, sometimes the icon glyph itself doesn't take up much room and ends up with too much whitespace around it. In this situation, use `.icon--narrow` to reduce the whitespace.

```blade render
<p>Text @icon('tabler-chevron-down') Text</p>
<p>Text @icon('tabler-chevron-down', ['class' => 'icon--narrow']) Text</p>
```

### Text Label

To pair an icon together with a text label, wrap them in an element with the `.with-icon` class:

```blade render
<span class="with-icon">
    @icon('tabler-star')
    Favorite
</span>
```

## Blade Directive

Use the `@icon` Blade directive to quickly emit icon markup. The value may be one of the following:

-   The name of a [Blade Icon](https://blade-ui-kit.com/blade-icons)
-   An emoji (prefixed with `emoji:`)
-   The path to a file in the public `icons` directory (prefixed with `file:`)

```blade render
@icon('tabler-star')
@icon('emoji:🐡')
```

You can pass an array of attributes to apply to the icon element as the second argument:

```blade render
@icon('tabler-star', ['class' => 'text-lg'])
```

## Icon Picker

Use the `<x-waterhole::cp.icon-picker>` component in a form to allow the user to choose an icon from a Blade Icon, Emoji, or an uploaded file. This component is only available in the context of the CP.

```blade render
<x-waterhole::cp.icon-picker
  name="icon"
  :value="old('icon')"
/>
```

This is best used in combination with a model with the `Waterhole\Models\Concerns\HasIcon` trait, which parses the icon picker value and saves it to the database/filesystem.
