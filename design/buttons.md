# Buttons
Buttons allow users to take actions and make choices with a single click.

The `.btn` class can be applied to any element to give it the appearance of a button:

```html render
<button class="btn">Button</button>
<a href="#" class="btn">Link Button</a>
```

## Colors
Use a `.bg` class to apply a color to the button:

```html render
<button class="btn bg-accent">Accent</button>
<button class="btn bg-success">Success</button>
<button class="btn bg-warning">Attention</button>
<button class="btn bg-danger">Danger</button>
<button class="btn bg-emphasis">Emphasis</button>
```

For toggle buttons, use the `[aria-pressed="true"]` attribute to indicate that the control is turned on:

```html render
<button class="btn" aria-pressed="true">Active</button>
```

## Variants
`.btn--outline` and `.btn--transparent` are available to create less visually prominent buttons:

```html render
<button class="btn btn--outline">Outline</button>
<button class="btn btn--transparent">Transparent</button>
```

## Icons
Icons can be used in buttons to supplement text labels:

```html render
<button class="btn">
    <x-waterhole::icon icon="tabler-check"/>
    Accept
</button>
```

Use the `.btn--icon` class to create an icon-only button. Don't forget to add an `aria-label` attribute or a [tooltip](./tooltips.md).

```html render
<button class="btn btn--icon" aria-label="Accept">
    <x-waterhole::icon icon="tabler-check"/>
</button>
```

## Sizes
`.btn--sm`, `.btn--wide` and `.btn--narrow` are available to tweak the size of a button:

```html render
<button class="btn btn--sm">Small</button>
<button class="btn">Normal</button>
<button class="btn btn--wide">Wide</button>
<button class="btn btn--narrow">Narrow</button>
```
