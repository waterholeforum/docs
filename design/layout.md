# Layout

Build consistent and flexible layouts for your customizations.

The Waterhole Design System facilitates an algorithmic approach to building layouts, inspired by [Every Layout](https://every-layout.dev/blog/algorithmic-design/) and [Build Excellent Websites](https://buildexcellentwebsit.es).

## Spacing

Underpinning the layout system is a modular spacing scale defined in `--space-{size}` CSS variables. It starts with an `md` value of `1rem` and then propagates by a factor of 1.5 down to `xxs` and up to `xxxl`:

```css
--ratio: 1.5;
--space-xxs: calc(var(--space-xs) / var(--ratio));
--space-xs: calc(var(--space-sm) / var(--ratio));
--space-sm: calc(var(--space-md) / var(--ratio));
--space-md: 1rem;
--space-lg: calc(var(--space-md) * var(--ratio));
--space-xl: calc(var(--space-lg) * var(--ratio));
--space-xxl: calc(var(--space-xl) * var(--ratio));
--space-xxxl: calc(var(--space-xxl) * var(--ratio));
```

There is also a `--space-gutter` variable which changes based on the viewport width:

```css
--space-gutter: min(2rem, 5vw);
```

## Container & Section

The `.container` class centers page content and applies horizontal gutter padding, while the `.section` class applies vertical gutter padding. These can be used together to construct basic page sections:

```html render
<section class="section">
    <div class="container">Content</div>
</section>

<section class="section bg-fill">
    <div class="container">Content</div>
</section>
```

## Row & Stack

`.row` and `.stack` set up a Flexbox context. `.row` arranges items horizontally with `center` alignment by default. `.stack` arranges items vertically with `stretch` alignment by default.

```html render
<div class="row gap-sm">
    <div class="card p-sm">Item 1</div>
    <div class="card p-sm">Item 2</div>

    <div class="stack gap-sm">
        <div class="card p-sm">Short</div>
        <div class="card p-sm">Loooooong</div>
    </div>
</div>
```

They can be used together with various utility classes to intuitively construct algorithmic flex layouts.

| Class              | CSS                               |
| ------------------ | --------------------------------- |
| `.gap-{size}`      | `gap: var(--space-{size})`        |
| `.gap-y-{size}`    | `row-gap: var(--space-{size})`    |
| `.gap-x-{size}`    | `column-gap: var(--space-{size})` |
| `.wrap`            | `flex-wrap: wrap`                 |
| `.wrap-reverse`    | `flex-wrap: wrap-reverse`         |
| `.justify-start`   | `justify-content: flex-start`     |
| `.justify-center`  | `justify-content: center`         |
| `.justify-end`     | `justify-content: flex-end`       |
| `.justify-between` | `justify-content: space-between`  |
| `.align-start`     | `align-items: flex-start`         |
| `.align-center`    | `align-items: center`             |
| `.align-end`       | `align-items: flex-end`           |
| `.align-baseline`  | `align-items: baseline`           |
| `.align-stretch`   | `align-items: stretch`            |
| `.dividers`        | Add a line between each item.     |

The following utility classes can be used on children:

| Class                  | CSS                                                                     |
| ---------------------- | ----------------------------------------------------------------------- |
| `.grow`                | `flex-grow: 1`                                                          |
| `.shrink`              | `min-width: 0`                                                          |
| `.no-shrink`           | `flex-shrink: 0`                                                        |
| `.align-self-center`   | `align-self: center`                                                    |
| `.align-self-start`    | `align-self: start`                                                     |
| `.align-self-end`      | `align-self: end`                                                       |
| `.align-self-baseline` | `align-self: baseline`                                                  |
| `.align-self-stretch`  | `align-self: stretch`                                                   |
| `.push-start`          | `margin-inline-end: auto`                                               |
| `.push-center`         | `margin-inline: auto`                                                   |
| `.push-end`            | `margin-inline-start: auto`                                             |
| `.break-sm`            | In a wrapping context, move the item onto its own row on small screens. |

Together these utilities provide a basis on which to construct responsive layouts, even without the use of breakpoints in many cases. They should be used in combination with semantic class names to apply additional specific rules as required. The below example demonstrates a responsive "hero" component:

```html render
<section class="hero row gap-xl wrap">
    <div class="hero__welcome grow stack gap-sm">
        <h1>Welcome</h1>
        <p>Welcome to the community.</p>
    </div>

    <aside class="hero__statistics grow row gap-lg justify-center">
        <div>123 Members</div>
        <div>12 Online</div>
        <div>1234 Posts</div>
    </aside>
</section>

<style>
    .hero__welcome {
        flex-basis: 20ch; /* items will wrap at this width */
    }
</style>
```

## Grid

The `.grid` utility class sets up a CSS Grid context with the following responsive column template:

```css
grid-template-columns: repeat(auto-fit, minmax(min(var(--grid-min, 15ch), 100%), 1fr));
```

This allows columns to be sized automatically according to the available space, and to wrap when a minimum size is reached. Most of the time it is necessary to use this in combination with a semantic class name to define a specific value for `--grid-min`:

```html render
<div class="demo-cards grid gap-sm">
    <div class="card p-sm">Item 1</div>
    <div class="card p-sm">Item 2</div>
    <div class="card p-sm">Item 3</div>
    <div class="card p-sm">Item 4</div>
</div>

<style>
    .demo-cards {
        --grid-min: 25ch;
    }
</style>
```

## Sidebar

A basic responsive "sidebar" layout can be constructed using the `.with-sidebar` class on a wrapper and `.sidebar` on the sidebar itself:

```html render
<section class="with-sidebar">
    <nav class="sidebar">Sidebar content</nav>

    <div class="card p-sm">Main content</div>
</section>
```

The sidebar can also be positioned on the right by making it the second child, after the main content.

The sidebar is a flexbox container. On larger screens when it is displayed on the side, children are stacked in a column; on small screens when it wraps above or below the main content, children are laid out in a row and may wrap.

The `.sidebar--sticky` class can be used to make the sidebar content stick to the top of the viewport as the user scrolls down.

For right-sided sidebars, the `.sidebar--bottom` class can be applied to make the sidebar stick to the bottom of the viewport on small screens.

## Overlay

The overlay utility is a quick way to make an element overlay a parent element, specified by `.overlay-container`.

Use the `.overlay` class to overlay the element itself, useful for overlaying text on an image, for example:

```html render
<div class="overlay-container">
    <img src="https://via.placeholder.com/600x200" alt="Image" class="block" />
    <div class="overlay">Overlay text</div>
</div>
```

Use the `.has-overlay` class to overlay a pseudo element, useful for making a link cover its entire card, for example:

```html render
<div class="card card__body overlay-container stack gap-sm">
    <h3>Card Title</h3>
    <p>Card Body</p>
    <a href="#" class="has-overlay">Read more</a>
</div>
```

## Breakpoints

For cases where algorithmic layout is not sufficient, Waterhole defines several standard breakpoints. They are:

| Breakpoint | Start   | End     |
| ---------- | ------- | ------- |
| `sm`       | `0`     | `40rem` |
| `md`       | `40rem` | `52rem` |
| `lg`       | `52rem` | `68rem` |
| `xl`       | `68rem` | `∞`     |

For the simple case of hiding elements at certain breakpoints, the following classes are available:

| Class           | Description                           |
| --------------- | ------------------------------------- |
| `.hide-sm`      | Hide in the `sm` breakpoint.          |
| `.hide-md-down` | Hide in the `md` breakpoint and down. |
| `.hide-md-up`   | Hide in the `md` breakpoint and up.   |
| `.hide-lg-down` | Hide in the `lg` breakpoint and down. |
| `.hide-lg-up`   | Hide in the `lg` breakpoint and up.   |

For any other breakpoint-dependent styling, you will need to use `@media` queries. If you're using PostCSS, you can import Waterhole's [`breakpoints.css` file](https://github.com/waterholeforum/core/blob/main/resources/css/system/breakpoints.css) and make use of the `@custom-media` queries defined in this file. Use the [Global Data](https://github.com/csstools/postcss-plugins/tree/main/plugins/postcss-global-data) plugin to make them globally available:

```js
// postcss.config.cjs
module.exports = {
    plugins: [
        require('@csstools/postcss-global-data')({
            files: ['./vendor/waterhole/core/resources/css/system/breakpoints.css'],
        }),
        require('postcss-preset-env'),
    ],
};
```
