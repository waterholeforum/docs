# Notifications

Waterhole builds on top of the [Laravel Notifications](https://laravel.com/docs/10.x/notifications) system to make implementing notifications even easier.

Waterhole takes care of all the boilerplate: managing user preferences, generating HTML emails, handling secure unsubscribe links, and rendering notifications. All you have to do is implement various methods that describe the notification content.

## Notification Types

To define a new Waterhole Notification type, begin by extending the [`Waterhole\Notifications\Notification` class](reference://Waterhole/Notifications/Notification.html):

```php
namespace App\Notifications;

use Waterhole\Notifications\Notification;
use Waterhole\Models\Comment;

class NewComment extends Notification
{
    // ...
}
```

### Content

The notification `content` is the model associated with the individual notification instance. **This must be the same as your notification's constructor argument.** It will be used to reconstruct the notification instance after the notification is read from the database:

```php
class NewComment extends Notification
{
    public function __construct(private Comment $comment)
    {
    }

    public function content(): Comment
    {
        return $this->comment;
    }
}
```

### Sender

The `sender` is the user whose action caused the notification to be sent. If present, the user's avatar will be displayed alongside the notification. In the case of a notification for a new comment, it is the author of the comment:

```php
public function sender(): ?User
{
    return $this->comment->user;
}
```

### Icon

Specify an `icon` to represent the notification. The icon may be a value accepted by the [`<x-waterhole::icon>` component](./design/icons.md#icon-component):

```php
public function icon(): string
{
    return 'tabler-chat';
}
```

### Title

Specify the `title` of the notification using Markdown syntax. This allows the notification to be expressed both in HTML and plain-text:

```php
public function title(): string
{
    return "New comment in **{$this->comment->post->title}**";
}
```

### Excerpt

You can include an `excerpt` from the notification content, which may include HTML:

```php
public function excerpt(): HtmlString
{
    return $this->comment->body_html;
}
```

### URL

Specify the `url` that the user should be taken to when they click the notification or the action button in the notification email:

```php
public function url(): string
{
    return $this->comment->post_url;
}
```

### Button

Specify the `button` text to be displayed on the action button in the notification email:

```php
public function button(): string
{
    return 'View Comment';
}
```

### Group

If multiple notifications should be grouped together by their subject, then you can define a `group` method. Here, even if there are multiple new comments on a particular post, they will be aggregated into a single notification on the user's notification list:

```php
public function group(): Model
{
    return $this->comment->post;
}
```

If the notification should link to a different URL when it is grouped, you can implement the `groupedUrl` method:

```php
public function groupedUrl(): string
{
    return $this->comment->post->unread_url;
}
```

### Reason

As a courtesy to the user, describe the `reason` that they have received the notification:

```php
public function reason(): string
{
    return 'You received this notification because you are following this post.';
}
```

### User Preference

To add a preference for your notification type to the user notification preferences page, first add a description of your notification type by implementing the static `description` method:

```php
public static function description(): string
{
    return 'New comments on followed posts';
}
```

Note that this is a **static** method, as it describes the notification type itself rather than a particular notification instance.

Then, register your notification type with the `NotificationTypes` extender in the boot method of a service provider:

```php
use Waterhole\Extend;

Extend\NotificationTypes::add('new-comment', NewComment::class);
```

### Unsubscribe Links

To allow the user to opt-out of receiving email notifications of this type, Waterhole will automatically generate and handle a secure unsubscribe link. If the user clicks the unsubscribe link, their preference for receiving emails for the notification type will be turned off.

You can customize the label of the unsubscribe link by implementing the `unsubscribeText` method:

```php
public function unsubscribeText(): string
{
    return 'Unsubscribe from comment notifications';
}
```

If you'd like to change the action of the unsubscribe link – for example, to make it "unfollow this post" – you can override the `unsubscribe` method as well:

```php
public function unsubscribeText(): string
{
    return 'Unfollow this post';
}

public function unsubscribe(User $user): void
{
    $this->comment->post->loadUserState($user)->unfollow();
}
```

### Eager Loading Content

When displaying the notification list, the `content` relationship will be eager-loaded. You may also eager-load additional relationships by implementing the static `load` method:

```php
public static function load(Collection $notifications): void
{
    $notifications->load('content.post', 'content.user');
}
```

## Sending Notifications

Refer to the [Laravel documentation](https://laravel.com/docs/10.x/notifications#sending-notifications) for information on how to send notifications.
