# Keyboard Shortcuts

Waterhole includes a keyboard shortcut system in the forum UI. Users can press
`Shift+?` to open the built-in shortcut reference, and extensions can add their
own shortcuts, bind them to UI elements, and opt components into keyboard
navigation.

Under the hood, Waterhole builds on the
[`inclusive-shortcuts`](https://www.npmjs.com/package/inclusive-shortcuts)
library. Its DOM-first approach is the key idea to keep in mind as you read this
page: shortcuts are defined in PHP, then resolved against real elements in your
markup.

## Registering Shortcuts

Use the
[`KeyboardShortcuts` extender](reference://Waterhole/Extend/Ui/KeyboardShortcuts.html)
in your service provider to add, remove, or reorder shortcuts:

```php
use Waterhole\Extend;
use Waterhole\Ui\KeyboardShortcut;

$this->extend(function (Extend\Ui\KeyboardShortcuts $shortcuts) {
    $shortcuts->add(
        new KeyboardShortcut(
            id: 'navigation.portal',
            keys: ['g c'],
            description: 'Go to the customer portal',
            category: 'navigation',
        ),
    );
});
```

Each shortcut has:

- An `id`, used to connect the shortcut to DOM elements and events.
- A `keys` array containing one or more bindings.
- A human-readable `description` shown in the shortcut reference.
- A `category`, used to group shortcuts in the reference dialog.
- Optional `scopes`, limiting where the shortcut is active.

Shortcut IDs must be unique. If you want to give a shortcut multiple bindings,
add more values to its `keys` array instead of registering the same ID twice.

Like other Waterhole extenders, items can also be added as closures. This is
useful when a shortcut should only exist for certain users or when some feature
is enabled:

```php
use Illuminate\Support\Facades\Auth;
use Waterhole\Extend;
use Waterhole\Ui\KeyboardShortcut;

$this->extend(function (Extend\Ui\KeyboardShortcuts $shortcuts) {
    $shortcuts->add(fn() => Auth::check()
        ? new KeyboardShortcut(
            id: 'navigation.portal',
            keys: ['g c'],
            description: 'Go to the customer portal',
            category: 'navigation',
        )
        : null);
});
```

## Key Bindings

Shortcut bindings use the syntax supported by `inclusive-shortcuts`. Some
examples from Waterhole core:

- `/` for a single key
- `g h` for a sequence
- `Shift+?` for a modified key
- `$mod+Enter` for a platform-aware modifier (`Ctrl` on Windows/Linux, `Cmd` on
  macOS)

## Triggering Elements

To make a shortcut activate an element, add `data-shortcut-trigger` with the
shortcut ID:

```blade
<a href="{{ route('portal') }}" data-shortcut-trigger="navigation.portal">
    Portal
</a>
```

When the shortcut fires, Waterhole will:

- Focus the target if it is a text input or another focusable non-click target.
- Click the target if it is a button, link, or other clickable control.

An element can respond to multiple shortcuts by separating IDs with spaces:

```blade
<button
    type="button"
    data-shortcut-trigger="editor.full-screen navigation.close"
>
    Close
</button>
```

## Scopes

By default, shortcuts are active anywhere inside the page root. To limit a
shortcut to part of the UI, pass `scopes` when creating it:

```php
new KeyboardShortcut(
    id: 'editor.portal-snippet',
    keys: ['$mod+Shift+P'],
    description: 'Insert portal snippet',
    category: 'editor',
    scopes: ['editor'],
);
```

Then mark a container with `data-shortcut-scope`:

```blade
<div data-shortcut-scope="editor">
    ...
</div>
```

Core uses scopes like `form`, `editor`, `surface`, `selection`, and `global`,
but custom scope names are fine too.

Waterhole also supports `data-shortcut-context` as a narrower hint inside the
current scope. When focus is inside a context container, matching triggers in
that context are preferred before falling back to the rest of the current scope.

## Hidden Content

Normally, shortcut resolution ignores hidden content. Add `data-shortcut-hidden`
to a subtree when you want it to keep participating in shortcut resolution even
while it is hidden.

This is useful for things like hidden menus, popovers, or dialogs that still
need to contribute shortcut targets or shortcut labels before they are shown.
Inert content is still excluded.

## Categories

The built-in shortcut reference groups shortcuts by category. Waterhole core
uses `navigation`, `discussion`, and `editor`.

If you introduce your own category, make sure it has a translated label for the
`keyboard-shortcuts-category-{category}` key so the reference page can display a
proper heading.

## Selection Navigation

Waterhole's discussion shortcuts (`j`, `k`, `o`, `a`, and so on) are built on a
selection system. To make a list keyboard-navigable, mark each item with a
selection key and identify the primary action inside it:

```blade
<article data-shortcut-selection-key="{{ dom_id($post) }}">
    <a
        href="{{ $post->url }}"
        data-shortcut-selection-primary
        data-shortcut-trigger="selection.open"
    >
        {{ $post->title }}
    </a>

    <x-waterhole::action-menu :for="$post" />
</article>
```

Useful attributes include:

- `data-shortcut-selection-key` on each selectable item.
- `data-shortcut-selection-primary` on the element that should receive focus and
  provide the selection announcement when that item becomes selected.
- `data-shortcut-selection-default` on the fallback item to use when the user
  has not explicitly selected anything yet.
- `data-shortcut-selection-owner` on related controls outside the selected item
  so they still resolve as belonging to that item.

## Listening For Shortcut Events

Before Waterhole runs a shortcut, it dispatches a cancelable
`waterhole:shortcut` event on `document`. You can listen for this to run custom
behavior or intercept an existing shortcut.

Prefer triggering a real element with `data-shortcut-trigger` when possible, so
the same action is available without the keyboard shortcut and can participate
in normal focus, tooltip, and accessibility behavior. Reach for the event only
when you need custom JavaScript behavior that does not map cleanly to an
existing control:

```js
document.addEventListener('waterhole:shortcut', (event) => {
    if (event.detail.id === 'navigation.portal') {
        event.preventDefault();
        window.location.href = '/portal';
    }
});
```

## Action Shortcuts

[Actions](./actions.md) can define a shortcut by returning a `KeyboardShortcut`
from their `shortcut()` method:

```php
use Illuminate\Support\Collection;
use Waterhole\Actions\Action;
use Waterhole\Ui\KeyboardShortcut;

class FeaturePost extends Action
{
    public function label(Collection $items): string
    {
        return 'Feature';
    }

    public function shortcut(): ?KeyboardShortcut
    {
        return new KeyboardShortcut(
            id: 'action.feature',
            keys: ['f'],
            description: 'Feature the selected post',
            category: 'discussion',
            scopes: ['selection'],
        );
    }
}
```

Waterhole will automatically expose the shortcut in action buttons and menus,
and include it in the shortcut reference for authorized users.

Most action shortcuts should include the `selection` scope so they apply to the
currently selected item.
