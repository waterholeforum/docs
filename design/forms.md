# Forms & Inputs

## Input

Use the `.input` class to style any element (`<input>`, `<textarea>`, and `<select>`) as a input:

```html render
<div class="stack gap-sm">
    <input type="text" class="input" value="Input" />

    <textarea class="input">Textarea</textarea>

    <select class="input">
        <option>Select</option>
    </select>

    <input type="file" class="input" />
</div>
```

### Input Container

Wrap an input in an `.input-container` to decorate it with icons or buttons:

```blade render
<div class="input-container">
  <x-waterhole::icon icon="tabler-search" class="no-pointer"/>
  <input type="text" class="input">
  <span>
      <button class="btn btn--icon btn--sm btn--transparent">
        <x-waterhole::icon icon="tabler-x"/>
      </button>
  </span>
</div>
```

## Checkbox & Radio

Checkboxes and radio inputs are globally styled to match the Waterhole theme.

Apply the `.choice` class to a surrounding `<label>` for proper alignment of a checkbox/radio input and its text label:

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

The text editor component decorates a `<textarea>` with a markdown toolbar, auto-suggests users for `@` mentions, and supports the ability to preview the formatted content:

```blade render
<x-waterhole::text-editor
  name="body"
  value="Hello, world!"
  placeholder="Enter your text"
  class="input"
/>
```

## Color Picker

Waterhole provides a color picker input component which allows the user to visually pick a hex (or hex alpha) color:

```blade render
<x-waterhole::admin.color-picker name="color" value="abcdef"/>
```

## Fields

Fields are a standard way to lay out a form control, its label, and help text or status message. If there is enough room, the label and control will be displayed in a row; otherwise, they will be stacked.

```html render
<div class="field">
    <label class="field__label">Email</label>
    <div class="grow stack gap-xs">
        <input type="email" class="input" />
        <div class="field__description">Enter your email address.</div>
    </div>
</div>
```

The `<x-waterhole::field>` Blade component can be used to efficiently construct this markup. It will also automatically show validation errors for the field:

```blade render
<x-waterhole::field
  name="email"
  label="Email"
  description="Enter your email address."
>
  <input type="email" name="email" class="input">
</x-waterhole::field>
```

Multiple fields can be spaced using the `.stack` layout utility – commonly with `.dividers`:

```blade render
<div class="stack dividers">
  <x-waterhole::field
    name="name"
    label="Name"
    description="Enter your full name."
  >
    <input type="text" name="name" class="input">
  </x-waterhole::field>

  <x-waterhole::field
    name="email"
    label="Email"
    description="Enter your email address."
  >
    <input type="email" name="email" class="input">
  </x-waterhole::field>
</div>
```
