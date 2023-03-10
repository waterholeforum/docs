# Cards
Cards are used to bound pieces of content.

The `.card` class creates a border around the content, while the `.card__body` class applies padding. Apply both of these to create a basic card:

```html render
<div class="card card__body">
  This is a card.
</div>
```

## Header
Add a card header using the `.card__header` class:

```html render
<div class="card">
  <div class="card__header">Header</div>
  <div class="card__body">Body</div>
</div>
```

These classes can be applied to `<details>` and `<summary>` elements to create a collapsible card:

```html render
<details class="card">
  <summary class="card__header">Header</summary>
  <div class="card__body">Body</div>
</details>
```

## Rows
To create a card containing a list or rows, use the `.card__row` class:

```html render
<ul class="card" role="list">
  <li class="card__row">
    Row 1
  </li>
  <li class="card__row">
    Row 2
  </li>
  <li class="card__row">
    Row 3
  </li>
</ul>
```