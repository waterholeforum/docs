# Forms

Waterhole allows you to easily add fields, validation, and persistence logic to forms throughout the application.

In Waterhole, a **form** is made up of an ordered list of **fields**. Each field is a Blade component with a `render` method, but it may also have additional methods which allow it to specify validation logic, and run code upon form submission, before and after a model is saved.

## Defining a Field

To define a new field, extend the `Waterhole\Forms\Field` class. The constructor will receive an instance of the model that is the subject of the form. A field is a Blade component, so you will need to define a `render` method that returns the field's view. Feel free to use the [`<x-waterhole::field>` component](./design/forms.md#fields) to take care of the boilerplate:

```php
namespace App\Fields;

use Waterhole\Forms\Field;

class FullNameField extends Field
{
    public function __construct(public ?User $user)
    {
    }

    public function render(): string
    {
        return <<<'blade'
            <x-waterhole::field name="full_name" label="Full Name">
                <input
                    type="text"
                    name="full_name"
                    value="{{ old('full_name', $user->full_name ?? '') }}"
                    class="input"
                >
            </x-waterhole::field>
        blade;
    }
}
```

### Validation

If your field's input will need to be validated, add a `validating` method. This method receives an instance of the Validator, to which you can add validation rules:

```php
use Illuminate\Validation\Validator;

public function validating(Validator $validator): void
{
    $validator->addRules([
        'full_name' => ['required', 'string', 'max:255'],
    ]);
}
```

If you need to specify custom validation logic, make sure you do so using the `after` method on the validator, which will allow you to add additional error messages to the message collection:

```php
$validator->after(function ($validator) {
    if ($this->somethingElseIsInvalid()) {
        $validator->errors()->add(
            'full_name', 'Something is wrong with the full name!'
        );
    }
});
```

### Saving

After the form has passed validation, the subject model will be saved. By implementing the `saving` method, fields are able to run code before the model is saved in order to assign validated data to the model:

```php
use Illuminate\Foundation\Http\FormRequest;

public function saving(FormRequest $request)
{
    $this->user->full_name = $request->validated('full_name');
}
```

Likewise, by implementing the `saved` method, fields can run code after the model has been saved:

```php
public function saved(FormRequest $request)
{
    logger("User {$this->user->id} was saved with a full name!");
}
```

## Form Structure

### Adding a Field to a Form

Most of the forms throughout Waterhole have a corresponding extender to which you can add fields. For example, to add a field to the registration form, we would use the [`RegistrationForm` extender](reference://Waterhole/Extend/Forms/RegistrationForm.html):

```php
use App\Fields\FullNameField;
use Waterhole\Extend;

$this->extend(function (Extend\Forms\RegistrationForm $form) {
    $form->add(FullNameField::class);
});
```

### Adding a Field to a Form Section

Some forms are more complex and group fields into distinct **sections**. One example is the form for editing channels, which divides fields into various collapsible sections.

Like their parent forms, most of the distinct form sections throughout Waterhole have a corresponding list to which you can add fields. For example, to add a field to the "Details" section of the channel editing form, we would use the `details` list on the `ChannelForm` extender:

```php
use App\Fields\ChannelColorField;
use Waterhole\Extend;

$this->extend(function (Extend\Forms\ChannelForm $form) {
    $form->details->add(ChannelColorField::class);
});
```

### Adding a Form Section

You can add a new section to a form using the [`FormSection` component](reference://Waterhole/Forms/FormSection.html), which can be constructed with a title and an array of field component instances. In the following example, we add a new "Membership" section to the channel editing form. Note how the `$channel` model is forwarded onto the field instances:

```php
use Waterhole\Extend;
use Waterhole\Forms\FormSection;
use Waterhole\Models\Channel;

$this->extend(function (Extend\Forms\ChannelForm $form) {
    $form->add(
        fn(Channel $channel) => new FormSection('Membership', [
            new MembershipEnabledField($channel),
            new MembershipPriceField($channel),
        ]),
    );
});
```

<!--
## Building New Forms
TODO
-->
