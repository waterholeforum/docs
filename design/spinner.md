# Spinner
Indicate that content is loading with a spinner.

## CSS
Use the `.spinner` class to display a spinner:

```html render
<div class="spinner" role="status" aria-label="loading"></div>
```

### Small
Use the `.spinner--sm` class to reduce the spinner size, making it suitable for use inside of other components:

```html render
<span role="status">
    <span class="spinner spinner--sm"></span>
    Loading...
</span>
```

### Block-level
To display the spinner as a block-level element with some padding, use `.spinner--block`:

```html render
<div class="spinner spinner--block" role="status" aria-label="loading"></div>
```

## Blade Component
Use the `<x-waterhole::spinner>` component to display a spinner, including the correct accessibility attributes:

```html render
<x-waterhole::spinner/>
```