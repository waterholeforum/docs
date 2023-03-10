# Badge
Badges are compact elements that indicate a particular state or association.

For example, badges are used to display user groups, post state like "locked", or the number of unread comments or notifications.

## Colors
The `.badge` class creates the basic style. It can be combined with a `.bg` class to give the badge color:

```html render
<span class="badge">12</span>
<span class="badge bg-accent">12</span>
<span class="badge bg-success">12</span>
<span class="badge bg-warning">12</span>
<span class="badge bg-danger">12</span>
<span class="badge bg-activity">12</span>
```

## Icons
Badges may have an icon as well as a label:

```html render
<span class="badge">
    <x-waterhole::icon icon="tabler-lock"/>
    Locked
</span>
```