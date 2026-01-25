# Dialog

A dialog encapsulates a specific task, like logging in, creating a post, or confirming an action.

## CSS

The `.dialog` class creates a border around the dialog, while the `.dialog__header` and `.dialog__body` class apply appropriate padding:

```html render
<div class="dialog" aria-labelledby="dialog-title">
    <div class="dialog__header">
        <h1 class="h3" id="dialog-title">My Dialog</h1>
    </div>

    <div class="dialog__body">Hello, world!</div>
</div>
```

### Sizes

By default, dialogs will size to fit their content. You can control the dialog width explicitly using the `.dialog--sm`:

```html render
<div class="dialog dialog--sm dialog__body">Small Dialog</div>
```

## Blade Component

As a shortcut for building dialog markup, you can use the `<x-waterhole::dialog>` component:

```blade render
<x-waterhole::dialog title="My Dialog">
    Hello, world!
</x-waterhole::dialog>
```

## Modals

Dialogs can be displayed modally (superimposed over the page) using the following pattern.

> **Tip:** If you are intending to use a modal dialog to confirm an action, it will be easier to use the [Actions](../actions.md) mechanism which takes care of this for you.

To open a dialog box in a modal overlay, first create a route and view that renders the dialog, wrapped in a [Turbo Frame](../frontend.md#turbo-frames) with `id="modal"`:

```php
Route::get('my-dialog', function () {
    return view('my-dialog');
});
```

```blade
<x-waterhole::layout title="My Dialog Title">
    <turbo-frame id="modal">
        <x-waterhole::dialog title="My Dialog Title">
            My Dialog Body
        </x-waterhole::dialog>
    </turbo-frame>
</x-waterhole::layout>
```

Then, create a link to your route, targeting the `modal` Turbo Frame with the attribute `data-turbo-frame="modal"`:

```blade
<a
    href="{{ url('my-dialog') }}"
    data-turbo-frame="modal"
>Open Modal</a>
```

There are a few ways to close the modal:

- Call the `modal#hide` Stimulus action inside the modal.

    ```blade
    <a href="{{ url()->previous() }}" data-action="modal#hide">Back</a>
    ```

- Return a response without a `<turbo-frame id="modal">` element.
- Return a [Turbo Stream](../frontend.md#turbo-streams) response.
