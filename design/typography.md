# Typography
Make use of consistent typographic styles for your content.

## Fonts
Waterhole defines three font families for use as CSS variables:

```css
/* The default family used for most text */
--font-text: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";

/* Display family for larger headings (no different by default) */
--font-display: var(--font-text);  

/* Monospace family used for code blocks */
--font-mono: SFMono-Regular, Menlo, Monaco, Consolas, "Courier New", monospace;
```

## Scale
Waterhole defines a typographic scale in `--text-{size}` CSS variables:

```css
--text-xxs: .8rem;
--text-xs: .875rem;
--text-sm: 1rem;
--text-md: 1.125rem;
--text-lg: 1.5rem;
--text-xl: 2rem;
--text-xxl: 2.5rem;
```

This scale can be directly applied to text using the `.text-{size}` utility classes:

```html render
<div class="text-xxs">.text-xxs</div>
<div class="text-xs">.text-xs</div>
<div class="text-sm">.text-sm</div>
<div class="text-md">.text-md</div>
<div class="text-lg">.text-lg</div>
<div class="text-xl">.text-xl</div>
<div class="text-xxl">.text-xxl</div>
```

## Line Height
Three line heights are defined, with the `default` being applied globally, the `condensed` value used for headings, and the `expanded` value used for [content](#content).

```css
--line-height-default: 1.3;
--line-height-condensed: 1.2;
--line-height-expanded: 1.5;
```

## Headings
Heading styles are applied to `<h1>`-`<h6>` elements globally.

```html render
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

These styles are also available as `.h1`-`.h6` classes, which are useful for changing the visual appearance while keeping the markup semantically correct:

```html render
<h2 class="h5">Looks like a heading 5, semantically a heading 2</h2>
```

## Content
User-generated content can be rendered inside a `.content` element to automatically apply styles and spacing to all semantic markup, including headings, quotes, lists, and code:

```html render
<div class="content">
  <h4>User Generated Content</h4>
  <blockquote>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
  </blockquote>
  <ul>
    <li>List</li>
    <li>Items</li>
  </ul>
</div>
```

## Weight
Three font weights are defined:

```css 
--weight-normal: 400;  
--weight-medium: 500;
--weight-bold: 600;
```

Use font weight utilities to apply these to text:

```html render
<div class="weight-normal">Normal</div>
<div class="weight-medium">Medium</div>
<div class="weight-bold">Bold</div>
```

## Alignment
Use text alignment utilities to left align, center, or right align text:

```html render
<p class="text-left">Left align</p>
<p class="text-center">Center</p>
<p class="text-right">Right align</p>
```

## Measure
Waterhole defines a [typographic measure](https://every-layout.dev/rudiments/axioms/), which is the maxiumum recommended width of the line of text to ensure readability. The measure is available in a variable and can be applied using a utility class:

```css
--measure: 80ch;
```

```html render
<p class="measure">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
```

## Links
Anchor elements are globally styled with the accent color, and an underline on hover. You can also use the `.link` class to apply matching styles to other elements and make them look like links.

```html render
<a href="#">Hyperlink</a>
<button class="link">Button Link</button>
```

Anchor elements in a `.content` block are given a persistent underline to improve accessibility.