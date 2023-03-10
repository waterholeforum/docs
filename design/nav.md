# Nav
A nav is a vertical list of navigation links, commonly presented in a sidebar.

## CSS
Use the `.nav` class for the containing element and `.nav-link` on the clickable elements, which may contain icons and badges. For accessibility, navs should be wrapped in a `<nav>` or element with `[role="navigation"]`.

```html render
<nav class="nav" aria-label="forum">
  <a href="#" class="nav-link" aria-current="page">
    <x-waterhole::icon icon="heroicon-o-home"/>
    <span>Home</span>
  </a>
  <a href="#" class="nav-link">
    <x-waterhole::icon icon="heroicon-o-home"/>
    <span>Home</span>
  </a>
</nav>
```

Nav links are regarded as "active" if they have the `.is-active` class or the `[aria-current="page"]` attribute.

Use the `.nav-heading` class for a heading:

```html render
<h3 class="nav-heading">Heading</h3>
```

## Blade Components
### Nav Link
The `<x-waterhole::nav-link>` component is available to easily construct nav links and conditionally apply the `[aria-current="page"]` attribute. In addition to a `label`, `icon`, `badge`, and `badge-class`, you can pass:

- `route` to link to a named route (active if that is the current route)
- `href` to link to a URL (active if the current URL starts with the given URL)
- `active` as a boolean or `Closure` to manually set the item's active state

```html render
<x-waterhole::nav-link
  label="Home"
  icon="tabler-home"
  badge="2"
  route="waterhole.home"
/>
```

### Nav Heading
The `<x-waterhole::nav-heading>` component can be used for headings.

```html render
<x-waterhole::nav-heading heading="Heading"/>
```

### Responsive Nav
In combination with the [sidebar layout](./layout.md#sidebar), the `<x-waterhole::responsive-nav>` component is great for constructing navs which collapse into a button at smaller screen sizes. 

It accepts an array of Blade Component instances which will be rendered in a nav. At smaller screen sizes, the nav will be hidden in a drawer, toggleable by a button. If there is an active `NavLink` component, its icon and label and will be displayed on the button; otherwise, it will be a generic "menu" button.

```html render
@php
    use Waterhole\View\Components\NavLink;
    
    $components = [
        new NavLink(label: 'Home', icon: 'tabler-home', route: 'waterhole.home'),
        new NavLink(label: 'Google', icon: 'tabler-google', href: 'https://google.com'),
    ];
@endphp

<div class="with-sidebar">
    <div class="sidebar">
        <x-waterhole::responsive-nav :components="$components"/>
    </div>

    <div>Content</div>
</div>
```

The component accepts the following optional slots:

- `empty` can be used to override the button content when no item is active
- `button` can be used to override the button content in all cases
