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
<x-waterhole::icon icon="tabler-star" class="icon--thin text-xl"/>
<x-waterhole::icon icon="tabler-star" class="icon--thick text-xl"/>
```

### Width

While icon elements are always square, sometimes the icon glyph itself doesn't take up much room and ends up with too much whitespace around it. In this situation, use `.icon--narrow` to reduce the whitespace.

```blade render
<p>Text <x-waterhole::icon icon="tabler-chevron-down"/> Text</p>
<p>Text <x-waterhole::icon icon="tabler-chevron-down" class="icon--narrow"/> Text</p>
```

### Text Label

To pair an icon together with a text label, wrap them in an element with the `.with-icon` class:

```blade render
<span class="with-icon">
    <x-waterhole::icon icon="tabler-star"/>
    Favorite
</span>
```

## Icon Component

Use the `<x-waterhole::icon>` component to quickly emit icon markup. The `icon` attribute may be one of the following:

- The name of a [Blade Icon](https://blade-ui-kit.com/blade-icons)
- An emoji (prefixed with `emoji:`)
- The path to a file in the public `icons` directory (prefixed with `file:`)

```blade render
<x-waterhole::icon icon="tabler-star"/>
<x-waterhole::icon icon="emoji:🐡"/>
```

## Icon Picker

Use the `<x-waterhole::icon-picker>` component in a form to allow the user to choose an icon from a Blade Icon, Emoji, or an uploaded file:

```blade render
<x-waterhole::admin.icon-picker
  name="icon"
  :value="old('icon')"
/>
```

This is best used in combination with a model with the `Waterhole\Models\Concerns\HasIcon` trait, which parses the icon picker value and saves it to the database/filesystem.
