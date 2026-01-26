# User Groups

Assign users to groups that elevate their permissions or add flair to their account.

User groups allow you to give users special permissions, like being able to see private content, or having moderation powers. They can also have an associated "badge" which is displayed to indicate a user is part of the group.

You can manage your community's user groups in the **Groups** section of the Control Panel. On a new installation, Waterhole automatically creates built-in groups:

- **Guest** and **Member** for baseline permissions
- **Admin** for full access
- **Mod** for moderation
- **Quarantine** to require approval for new users

## Group Badges

Newly created groups are invisible to non-admins – that is, no one (not even the users in the group) can see that a group exists or who has been assigned to it.

If you would like to expose a group so that it is publicly visible, you can check the **show this group as a user badge** option. You can customize the color of the badge an optionally add an icon. Group badges are shown on user profiles and posts/comments.

^^^
![](images/groups-badges.png){width=258 height=67}
^^^ Groups can be exposed as badges. Groups that appear with a dotted outline are only visible to admins.

## Permissions

Waterhole has a powerful permissions system that allows you to control access and abilities in different parts of your community.

Permissions can be managed from two perspectives:

- When editing a [structure](./structure.md) node (channel, page, or link), you can choose which user groups have permission to see the node. For channels you can also choose which user groups are allowed to post, comment, and moderate the channel. This is the best way to restrict access to a specific area of your community.

- When editing a user group, you can choose which structure nodes the group has permission to see and act on. This is useful if you want to grant global permissions to a particular user group (such as global moderation powers).

Waterhole also includes a **Suspend users** permission, which allows trusted
groups to suspend users from participating.

### Admin

There is a special user group called **Admin**. Users assigned to this group have globally elevated permissions – they can view all content and perform all actions. As such, you should only assign this group to people you trust.

## Auto-assign and Approval Rules

Groups can be set to **auto-assign** new users. They can also apply **approval
rules** to require moderator approval for posts/comments, and optionally remove
users from the group once their content is approved.

The Quarantine group is configured this way by default. See [Moderation](./moderation.md)
for how approval and the moderation queue work.

## Assigning Users

To assign a user to a group, go to the user's profile (or find them in the **Users** section of the Control Panel) and click Controls → Edit. Then, check the boxes next to the groups you want them to be in.
