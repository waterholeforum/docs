# Alerts

Alerts provide contextual feedback to the user.

Alerts may include an icon and action buttons. They can be shown inline or as a
toast.

## CSS

The `.alert` class is combined with a `.bg` color class to create the alert
container. `.alert__icon`, `.alert__message`, and `.alert__actions` lay out the
contents of the alert.

```blade render
<div class="alert bg-success">
    <span class="alert__icon">@icon('tabler-check')</span>
    <p>Action successful.</p>
    <span class="alert__actions">
        <button class="btn">Done</button>
    </span>
</div>
```

## Blade Component

Use the
[`<x-waterhole::alert>` component](reference://Waterhole/View/Components/Alert.php)
to render an alert in a Blade template. Pass a `type`, which corresponds to the
name of one of the `.bg` color classes. An icon will be selected automatically:

```blade render
<div class="stack gap-sm">
    <x-waterhole::alert type="success">The action was successful.</x-waterhole::alert>
    <x-waterhole::alert type="success-soft">The action was successful.</x-waterhole::alert>
    <x-waterhole::alert type="warning">Something needs your attention.</x-waterhole::alert>
    <x-waterhole::alert type="warning-soft">Something needs your attention.</x-waterhole::alert>
    <x-waterhole::alert type="danger">There was an error.</x-waterhole::alert>
    <x-waterhole::alert type="danger-soft">There was an error.</x-waterhole::alert>
</div>
```

### Icon

You can pass the name of an `icon` to use instead of the default:

```blade render
<x-waterhole::alert type="danger" icon="tabler-circle-minus">
    Access denied.
</x-waterhole::alert>
```

### Actions

Alerts can be made dismissible by adding the `dismissible` property:

```blade render
<x-waterhole::alert type="danger" dismissible>
    Make me go away, I dare you.
</x-waterhole::alert>
```

You can also render a custom action in the `action` slot:

```blade render
<x-waterhole::alert type="warning" dismissible>
    Log in is required for this section.
    <x-slot:action>
        <a href="#" class="btn">Log In</a>
    </x-slot>
</x-waterhole::alert>
```

## Toasts

Waterhole uses an
[inclusive Alerts element](https://github.com/tobyzerner/inclusive-elements/tree/master/src/alerts)
to manage accessible toast messages displayed at the top of the page. If you
need to interact directly with this element, you can obtain a reference to it
via `window.Waterhole.alerts`.

You can show simple toast messages (`success`, `warning`, and `danger` messages)
by setting Laravel session `flash` data:

```php
session()->flash('success', 'Task was successful!');
session()->flash('warning', 'Something needs your attention.');
session()->flash('danger', 'There was an error.');
```
