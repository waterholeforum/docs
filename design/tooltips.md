# Tooltips

Tooltips display informative text when users hover over, focus on, or tap an element.

Waterhole uses the [inclusive tooltip element](https://github.com/tobyzerner/inclusive-elements/tree/master/src/tooltip) for tooltips. `.visually-hidden` styles are automatically applied to the `<ui-tooltip>` element.

```blade render
<!-- Tooltip as primary label -->
<button class="btn btn--icon">
  <x-waterhole::icon icon="tabler-bookmark"/>
  <ui-tooltip>Bookmarks</ui-tooltip>
</button>

<!-- Tooltip as auxiliary description -->
<button class="btn" aria-describedby="settings-description">
  <x-waterhole::icon icon="tabler-settings"/>
  Settings
  <ui-tooltip id="settings-description" hidden>
    View and manage settings
  </ui-tooltip>
</button>
```
