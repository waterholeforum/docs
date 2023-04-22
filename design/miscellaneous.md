# Misceallenous

Miscellaneous design tokens and utilities for use in the design system.

## Border Radius

Waterhole defines a global border radius variable:

```css
--radius: 10px;
```

The `.rounded` utility class will apply this border radius, while `.pill` will apply maximum rounding:

```html render
<div class="stack gap-md">
    <div class="rounded bg-fill p-sm">.rounded</div>
    <input type="text" class="pill input" value=".pill" />
</div>
```

## Box Shadow

Waterhole defines two levels of shadow:

| Variable      | Usage                                                  |
| ------------- | ------------------------------------------------------ |
| `--shadow-sm` | Use to show subtle layering, like a sticky header.     |
| `--shadow-md` | Use to show more overt layering, like a dropdown menu. |

```html render
<div class="row gap-md">
    <div class="rounded p-sm" style="box-shadow: var(--shadow-sm)">shadow-sm</div>
    <div class="rounded p-sm" style="box-shadow: var(--shadow-md)">shadow-md</div>
</div>
```

## Z-index

Z-index values are defined to ensure proper layering of elements:

| Variables           | Usage                                                                 |
| ------------------- | --------------------------------------------------------------------- |
| `--z-index-header`  | The Z-index of the page header.                                       |
| `--z-index-overlay` | The Z-index of overlay elements like dropdowns, tooltips, and modals. |
| `--z-index-alerts`  | The Z-index of the alerts container.                                  |

## Utilities

Waterhole includes various other utilities that may be useful:

| Class                     | Description                                                                   |
| ------------------------- | ----------------------------------------------------------------------------- |
| `.js-only`                | Hidden if JavaScript is disabled.                                             |
| `.no-js-only`             | Hidden if JavaScript is enabled.                                              |
| `.light-only`             | Hidden in dark mode.                                                          |
| `.dark-only`              | Hidden in light mode.                                                         |
| `.hide-if-empty`          | Hidden if the element is `:empty`.                                            |
| `.hide-if-not-only-child` | Hidden if the element has siblings.                                           |
| `.visually-hidden`        | Visually hide content that should remain available to assistive technologies. |
| `.no-pointer`             | Prevent the element from responding to pointer events.                        |
| `.no-select`              | Prevent text from being selected.                                             |
| `.clickable`              | Apply `cursor: pointer`, hover, and click effects.                            |
| `.cursor-default`         | Apply the default cursor to the element.                                      |
| `.cursor-help`            | Apply the help cursor to the element.                                         |
| `.cursor-pointer`         | Apply the pointer cursor to the element.                                      |
| `.full-width`             | Give an element `width: 100%`.                                                |
| `.full-height`            | Give an element `height: 100%`.                                               |
| `.rotate-90`              | Rotate an element by 90 degrees.                                              |
| `.rotate-180`             | Rotate an element by 180 degrees.                                             |
| `.rotate-270`             | Rotate an element by 270 degrees.                                             |
| `.flip-horizontal`        | Flip the element horizontally.                                                |
| `.flip-vertical`          | Flip the element vertically.                                                  |
| `.nowrap`                 | Disable text and flex wrapping.                                               |
| `.block`                  | `display: block` and `width: 100%`.                                           |
| `.inline`                 | `display: inline` and `vertical-align: middle`.                               |
| `.float-left`             | `float: left`                                                                 |
| `.float-right`            | `float: right`                                                                |
| `.scrollable`             | `overflow: auto` and hide the scrollbars.                                     |
| `.overflow-visible`       | `overflow: visible`                                                           |
| `.overflow-hidden`        | `overflow: hidden`                                                            |
| `.overflow-ellipsis`      | Disable text wrapping and truncate overflowing text.                          |
