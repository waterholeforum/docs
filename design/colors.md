# Colors

Use variables and utilities to apply consistent and accessible color.

## Variable System

There are two sets of color variables:

- The `--palette-*` variables are *constant* and define all of the available colors. These are set according to the theme (light or dark) and are where top-level customizations should be applied. Generally they should not be consumed directly by components (use the `color` variables instead).

- The `--color-*` variables are *dynamic* and define the color scheme in the current context. By default these variables are set to their palette counterparts, but they can be overridden at an element-level to influence how children are styled. For example, if an element is styled with a dark background, it may also override the `--color-text` variable so that any children which consume this color will adapt.

<details class="card">
<summary class="card__header">Quick illustration of this system</summary>

```css
/* The themes define the palette variables */
[data-theme='light'] {
    --palette-bg: white;
    --palette-text: black;
}

[data-theme='dark'] {
    --palette-bg: black;
    --palette-text: white;
}

/* The color variables default to the same */
:root {
    --color-bg: var(--palette-bg);
    --color-text: var(--palette-text);
}

/* Components consume color variables */
.btn {
    background: var(--color-bg);
    color: var(--color-text);
}

/* An element might override them, so that any child components will adapt */
.bg-dark {
    --color-bg: var(--palette-text);
    --color-text: var(--palette-bg);
}
```
</details>

## Palette

Waterhole's color palette consists of:

-   **Base colors** to help define user interface structure.
-   Several **functional colors** to convey interactivity and meaning (accent, success, warning, danger, activity).

All colors are selected to pass a minimum WCAG accessibility rating of [level AA](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html). Meeting these standards ensures that content is accessible by everyone, regardless of ability or device.

### Base Colors

| Color                                                                                                       | Usage                                                                 |
| ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| <span class="swatch" style="background: var(--color-bg)"></span> `--color-bg`                               | Page background color.                                                |
| <span class="swatch" style="background: var(--color-text)"></span> `--color-text`                           | Default color for text and icons.                                     |
| <span class="swatch" style="background: var(--color-muted)"></span> `--color-muted`                         | Secondary color for text and icons.                                   |
| <span class="swatch" style="background: var(--color-fill)"></span> `--color-fill`                           | Provides subtle contrast against a background.                        |
| <span class="swatch" style="background: var(--color-stroke)"></span> `--color-stroke`                       | Use for borders or dividers.                                          |
| <span class="swatch" style="background: var(--color-emphasis)"></span> `--color-emphasis`                   | Highest contrast against the default background, such as in tooltips. |
| <span class="swatch" style="background: var(--color-emphasis-contrast)"></span> `--color-emphasis-contrast` | Optimal contrast against the emphasis color.                          |

Both `--color-fill` and `--color-stroke` are semi-transparent colors so they can be layered:

```html render
<div class="bg-fill p-md">
    <div class="bg-fill p-md"></div>
</div>
```

### Functional Colors

Functional colors are used to convey interactivity and meaning. The functional colors are:

| Color                                               | Usage                                                       |
| --------------------------------------------------- | ----------------------------------------------------------- |
| <span class="swatch bg-accent"></span> `accent`     | Primary brand color used to convey interactivity and state. |
| <span class="swatch bg-success"></span> `success`   | Emphasize a positive message.                               |
| <span class="swatch bg-warning"></span> `warning`   | Emphasize elements that require a user's attention.         |
| <span class="swatch bg-danger"></span> `danger`     | Emphasize an error message or destructive action.           |
| <span class="swatch bg-activity"></span> `activity` | Emphasize new activity of interest to the user.             |

Each functional color has four associated varieties:

| Variable                                                                                                | Usage                                                                    |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| <span class="swatch bg-accent"></span> `--color-{name}`                                                 | Background color for elements like buttons and alerts.                   |
| <span class="swatch" style="background: var(--color-accent-contrast)"></span> `--color-{name}-contrast` | Optimal contrast against the background color.                           |
| <span class="swatch bg-accent-soft"></span> `--color-{name}-soft`                                       | Soft background color to subtly highlight an element.                    |
| <span class="swatch" style="background: var(--color-accent-text)"></span> `--color-{name}-text`         | Optimal contrast against the soft color (and the page background color). |

## Utilities

### Foreground Color

Use the `.color` utilities to set text or icons to a specific color:

```html render
<div class="color-text">.color-text</div>
<div class="color-muted">.color-muted</div>
<div class="color-accent">.color-accent</div>
<div class="color-success">.color-success</div>
<div class="color-warning">.color-warning</div>
<div class="color-danger">.color-danger</div>
<div class="color-activity">.color-activity</div>
```

The `.color-inherit` utility is available to set color inheritance:

```html render
<div class="color-success">
    This text is green, <a href="#" class="color-inherit underline">including the link</a>
</div>
```

### Background Color

Use the `.bg` utilities to set an element's background to a specific color. They will also set the element's foreground color to provide optimal contrast:

```html render
<div class="bg-fill p-sm">.bg-fill</div>
<div class="bg-emphasis p-sm">.bg-emphasis</div>
<div class="bg-accent p-sm">.bg-accent</div>
<div class="bg-accent-soft p-sm">.bg-accent-soft</div>
<div class="bg-success p-sm">.bg-success</div>
<div class="bg-success-soft p-sm">.bg-success-soft</div>
<div class="bg-warning p-sm">.bg-warning</div>
<div class="bg-warning-soft p-sm">.bg-warning-soft</div>
<div class="bg-danger p-sm">.bg-danger</div>
<div class="bg-danger-soft p-sm">.bg-danger-soft</div>
<div class="bg-activity p-sm">.bg-activity</div>
<div class="bg-activity-soft p-sm">.bg-activity-soft</div>
```
