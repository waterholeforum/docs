# Menu

Display a list of choices or commands in a dropdown menu.

Waterhole uses the inclusive [popup](https://github.com/tobyzerner/inclusive-elements/tree/master/src/popup) and [menu](https://github.com/tobyzerner/inclusive-elements/tree/master/src/menu) elements to power menu buttons. Wrap the trigger button and menu in a `<ui-popup>` element to hook it up as an accessible popup. Use a `<ui-menu>` element for the menu itself to enable keyboard navigation.

Menus should be styled with the `.menu` class. To prevent [FOUC](https://en.wikipedia.org/wiki/Flash_of_unstyled_content), add the `hidden` attribute to the menu so it is hidden even before the JavaScript has loaded.

Use the `.menu-item` class for menu items, which can be buttons, links, or labels. Menu items support icons.

```blade render
<ui-popup placement="bottom-start">
  <button class="btn">
    Controls
    <x-waterhole::icon icon="tabler-chevron-down"/>
  </button>

  <ui-menu class="menu" hidden>
    <button class="menu-item" role="menuitem">
      <x-waterhole::icon icon="tabler-star"/>
      Star
    </button>

    <button class="menu-item color-danger" role="menuitem">
      <x-waterhole::icon icon="tabler-trash"/>
      Delete...
    </button>
  </ui-menu>
</ui-popup>
```

## Divider

Use `.menu-divider` to divide groups of related items.

```html render
<ui-menu class="menu">
  <button class="menu-item" role="menuitem">Menu Item</button>
  <hr class="menu-divider" />
  <button class="menu-item" role="menuitem">Menu Item</button>
</ui-menu>
```

## Headings

Use the `.menu-heading` class for group headings.

```html render
<ui-menu class="menu">
  <h4 class="menu-heading">Heading</h4>
  <button class="menu-item" role="menuitem">Menu Item</button>
</ui-menu>
```

## Title & Description

Menu items may have a title and description, rather than just a text label. Use `.menu-item__title` and `.menu-item__description` for this:

```blade render
<ui-menu class="menu">
  <button class="menu-item" role="menuitem">
    <x-waterhole::icon icon="tabler-star"/>
    <span>
      <span class="menu-item__title">Follow</span>
      <span class="menu-item__description">Get notifications about new comments.</span>
    </span>
  </button>
</ui-menu>
```

## Selected

Menu items will be styled as "selected" if they have the `.is-active` class, or `[aria-current="page"]` or `[aria-checked="true"]` attributes. The `.menu-item__check` class can be used to display an icon which will only become visible if the containing menu item is selected.

```blade render
<ui-menu class="menu">
  <button class="menu-item" role="menuitemradio" aria-checked="true">
    <span>Menu Item</span>
    <x-waterhole::icon icon="tabler-check" class="menu-item__check"/>
  </button>
  <button class="menu-item" role="menuitem">Menu Item</button>
</ui-menu>
```
