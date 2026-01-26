# Forms & Inputs

## Input

`<input>`, `<textarea>`, and `<select>` elements are styled globally. You can
apply identical styles to other elements using the `.input`, `.textarea`, and
`.select` classes:

```html render
<div class="stack gap-sm">
    <input type="text" value="Input" />

    <textarea>Textarea</textarea>

    <select>
        <option>Select</option>
    </select>

    <input type="file" />

    <div class="input">Div that looks like an input</div>
</div>
```

### Input Container

Wrap an input in an `.input-container` to decorate it with icons or buttons:

```blade render
<div class="input-container">
    @icon('tabler-search', ['class' => 'no-pointer'])
    <input type="text">
    <span>
        <button class="btn btn--icon btn--sm btn--transparent">
            @icon('tabler-x')
        </button>
    </span>
</div>
```

## Checkbox & Radio

Checkboxes and radio inputs are styled globally.

Apply the `.choice` class to a surrounding `<label>` for proper alignment of a
checkbox/radio input and its text label:

```html render
<div class="stack gap-sm">
    <label class="choice">
        <input type="checkbox" />
        Checkbox
    </label>

    <label class="choice">
        <input type="radio" />
        Radio
    </label>
</div>
```

## Text Editor

The
[TextEditor component](reference://Waterhole/View/Components/TextEditor.html)
decorates a `<textarea>` with a markdown toolbar, auto-suggests users for `@`
mentions, and supports the ability to preview the formatted content:

```blade render
<x-waterhole::text-editor
    name="body"
    value="Hello, world!"
    placeholder="Enter your text"
    class="input"
/>
```

## Color Picker

The
[ColorPicker component](reference://Waterhole/View/Components/Cp/ColorPicker.html)
allows the user to visually pick a hex (or hex alpha) color. This component is
only available in the context of the CP.

```blade render
<x-waterhole::cp.color-picker name="color" value="abcdef"/>
```

## Fields

Fields are a standard way to lay out a form control, its label, and help text or
status message. If there is enough room, the label and control will be displayed
in a row; otherwise, they will be stacked.

```html render
<div class="field">
    <label class="field__label">Email</label>
    <div class="grow stack gap-xs">
        <input type="email" />
        <div class="field__description">Enter your email address.</div>
    </div>
</div>
```

The [Field component](reference://Waterhole/View/Components/Field.html) can be
used to efficiently construct this markup. It will also automatically show
validation errors for the field:

```blade render
<x-waterhole::field
    name="email"
    label="Email"
    description="Enter your email address."
>
    <input type="email" name="email">
</x-waterhole::field>
```

Multiple fields can be spaced using the `.stack` layout utility – commonly
together with `.dividers`:

```blade render
<div class="stack dividers">
    <x-waterhole::field
        name="name"
        label="Name"
        description="Enter your full name."
    >
        <input type="text" name="name">
    </x-waterhole::field>

    <x-waterhole::field
        name="email"
        label="Email"
        description="Enter your email address."
    >
        <input type="email" name="email">
    </x-waterhole::field>
</div>
```
