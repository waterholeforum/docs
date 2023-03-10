# Tabs

Tabs are a horizontal list of navigation links, used to switch between views.

Use the `.tabs` class for the containing element and `.tab` on the clickable elements. For accessibility, tabs should be wrapped in a `<nav>` or element with `[role="navigation"]`.

```html render
<nav aria-label="example">
  <ul class="tabs" role="list">
    <li>
      <a href="#" class="tab" aria-current="page">Tab 1</a>
    </li>
    <li>
      <a href="#" class="tab">Tab 2</a>
    </li>
    <li>
      <a href="#" class="tab">Tab 3</a>
    </li>
  </ul>
</nav>
```

A tab is regarded as "active" if it has the `.is-active` class or the `[aria-current="page"]` attribute.
