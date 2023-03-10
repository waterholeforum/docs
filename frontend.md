# Frontend

Waterhole's frontend is made up of Laravel Blade views and components, and uses [Hotwire](https://hotwired.dev) to coordinate partial page updates.

## Blade Components

Waterhole exposes many Blade Components that you can use in your own views. These are documented in the [`Waterhole\View\Components` namespace](https://waterhole.dev/docs/reference/Waterhole/View/Components.html).

Perhaps the most important of these to know is the [`<x-waterhole::layout>` component](https://waterhole.dev/docs/reference/Waterhole/View/Components/Layout.html). Use this to render your own views inside the Waterhole layout:

```blade
<x-waterhole::layout title="Hello World">
    <h1>Hello, world!</h1>
</x-waterhole::layout>
```

Blade Components are only used to encapsulate more complex HTML structures and behavior. For basic UI elements like buttons and inputs, use plain HTML elements and apply Waterhole's [CSS classes](./design/overview.md) directly.

## Component Lists

Many parts of Waterhole's templates render **lists** of components. For example, the page header is made up of these components:

- The forum title
- A "spacer" element
- The search button
- The notifications button
- The user menu

You can hook into these component lists and add your own components, or remove existing ones. All of these component lists are exposed as [extenders](./extending.md#extenders).

Check out the [list of extenders](https://waterhole.dev/docs/reference/Waterhole/Extend.html) to learn about all the points where you can modify components. The documentation for each extender also describes what parameters will be passed into the components when they're rendered.

### Adding Components

To add a component, call the `add` method on the applicable extender in the boot method of a service provider:

```php
use Waterhole\Extend;
use App\Views\Components\HelloWorld;

Extend\SiteHeader::add(HelloWorld::class);
```

The component can be any one of the following:

- The name of a Blade Component class (eg. `HelloWorld::class`)
- A Blade Component instance (eg. `new HelloWorld()`)
- The name of a view (eg. `waterhole.example`)
- A callable that returns any of the above

### Component Positions

Each item in a component list has a "position", and components are rendered by their position in ascending order. To specify a position, pass it as the third argument. Otherwise, a default of `0` will be used.

```php
Extend\SiteHeader::add(HelloWorld::class, position: 10);
```

### Component Keys

When adding a component to an extender, you can specify a unique key. This will allow other extensions to replace the component by using the same key, or remove it:

```php
Extend\SiteHeader::add(HelloWorld::class, key: 'hello');

// Replace the existing `hello` component
Extend\SiteHeader::add(HelloWorld::class, key: 'hello');

// Remove the `hello` component
Extend\SiteHeader::remove('hello');
```

### Rendering Component Lists

If you're building an extension, you can expose your own extendable component lists. First, create an extender using the `Waterhole\Extend\Concerns\OrderedList` and `OfComponents` traits and add any default components:

```php
namespace Acme\Example\Extend;

use Waterhole\Extend\Concerns\OrderedList;
use Waterhole\Extend\Concerns\OfComponents;

class PortalHeader
{
    use OrderedList, OfComponents;
}

PortalHeader::add(PortalTitle::class, -10);
```

Now, in your view, you can retrive the ordered component instances using the `components` method of your extender (passing in any props), and then use the `@components` Blade directive to render them:

```blade
@components(Acme\Example\Extend\PortalHeader::components(['foo' => 'bar']))
```

## Hotwire

Waterhole uses [Hotwire](https://hotwired.dev) to achieve the speed and responsiveness of a single-page application, while keeping template rendering on the server.

The [turbo-laravel](https://github.com/tonysm/turbo-laravel) library is included and can be used to help mark-up Turbo Frames and build Turbo Stream responses.

### Stimulus & Custom Elements

Waterhole's templates are all server-rendered HTML, progressively enhanced with sprinklings of JavaScript via both [Stimulus](https://stimulus.hotwired.dev) and [Custom Elements](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements).

### Turbo Drive

[Turbo Drive](https://turbo.hotwired.dev/handbook/drive) accelerates links and form submissions by negating the need for full page reloads. It is enabled by default in all Waterhole views. If needed, you can [opt out](https://turbo.hotwired.dev/handbook/drive#disabling-turbo-drive-on-specific-links-or-forms) on individual links and forms.

### Turbo Frames

[Turbo Frames](https://turbo.hotwired.dev/handbook/frames) allow predefined parts of a page to be updated on request. They are used in various places throughout the Waterhole UI. For example, a `<turbo-frame>` wraps each comment so that when you click "Like" or "Edit", only that comment will be re-rendered.

You may need to be mindful of Turbo Frames when hooking into Waterhole's views. It is possible to force a link or form to ["break out" of a frame](https://turbo.hotwired.dev/handbook/frames#targeting-navigation-into-or-out-of-a-frame) if needed.

### Turbo Streams

[Turbo Streams](https://turbo.hotwired.dev/handbook/streams) are fragments of HTML wrapped in `<turbo-stream>` elements that specify where and how the HTML should be merged into the page. Waterhole uses Turbo Streams to deliver partial page updates after running [Actions](./actions.md), and to broadcast real-time updates through WebSockets.

#### Streamable Components

Waterhole includes a mechanism to simplify the process of rendering a Blade Component in a Turbo Stream. First, the component must include the [`Waterhole\View\Components\Concerns\Streamable` trait]()(https://waterhole.dev/docs/reference/Waterhole/View/Components/Concerns/Streamable.html):

```php
use Illuminate\View\Component;
use Waterhole\Models\Post;
use Waterhole\View\Components\Concerns\Streamable;

class PostTitle extends Component
{
    use Streamable;

    public function __construct(public Post $post)
    {
    }

    public function render()
    {
        return <<<blade
            <div {{ $attributes }}>
                {{ $post->title }}
            </div>
        blade;
    }
}
```

This trait exposes an `id()` method on the component, and the `$attributes` bag will automatically be populated with an `id` attribute using this value. By default, the ID will be derived from the component name and the component's first public property (`$post` in this example). You can override the method if you'd like to specify a custom ID.

To render a `<turbo-stream>` element for the streamable component, use the `Waterhole\Views\TurboStream` class. There is a method for each available Turbo Stream action. Pass in an instance of the streamable component:

```php
use Waterhole\Views\TurboStream;

TurboStream::replace(new PostTitle($post));
TurboStream::remove(new PostTitle($post));
TurboStream::append(new PostTitle($post), 'target_id');
TurboStream::prepend(new PostTitle($post), 'target_id');
TurboStream::before(new PostTitle($post), 'target_id');
TurboStream::after(new PostTitle($post), 'target_id');
```
