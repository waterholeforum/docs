# Breadcrumb

Indicate the current page's location within a navigational hierarchy that
automatically adds separators via CSS.

A standalone breadcrumb can be wrapped in a `<nav>`, with `aria-current="page"`
being used to mark the current page:

```html render
<nav aria-label="breadcrumb">
    <ol class="breadcrumb">
        <li><a href="#">Home</a></li>
        <li><a href="#">Section</a></li>
        <li><a href="#" aria-current="page">Current</a></li>
    </ol>
</nav>
```

The breadcrumb can also be used inside a `<header>` to give context to the
current page heading. An extra `<li>` without content is used to append a
separator to the end of the breadcrumb:

```html render
<header class="stack gap-xs">
    <ol class="breadcrumb">
        <li><a href="#">Home</a></li>
        <li aria-hidden="true"></li>
    </ol>
    <h1 class="h2">Current</h1>
</header>
```
