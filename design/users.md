# Users
Various components are included for working with users.

## Avatar
Render a user's avatar using the `<x-waterhole::avatar>` component. If the `user` is null, an "anonymous" avatar will be emitted. Add the `link` property to wrap the avatar in a link to the user's profile.

```blade render
<x-waterhole::avatar :user="$user" link style="width: 48px"/>
```

## User Link
Render a link to the user's profile using the `<x-waterhole::user-link>` component.

```blade render
<x-waterhole::user-link :user="$user">
    Link to {{ $user->name }}'s profile
</x-waterhole::user-link>
```

## User Label
Render a user's avatar and name using the `<x-waterhole::user-label>` component. Add the `link` property to render as a link to the user's profile.
```blade render
<x-waterhole::user-label :user="$user" link/>
```

## Group Badge
Render a group badge using the `<x-waterhole::group-badge>` component.
```blade render
<x-waterhole::group-badge :group="$group"/>
```

## Attribution
The `<x-waterhole::attribution>` component renders a user's avatar, name, groups, headline, and a date. This is used when displaying posts and comments.

```blade render
<x-waterhole::attribution
  :user="$user"
  :date="now()"
/>
```
