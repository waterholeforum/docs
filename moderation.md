# Moderation

Tools and settings for keeping your community healthy.

## Reporting and Flags

Members can report posts and comments. Reports create "flags" that appear in the
moderation queue.

Preset report reasons can be configured in `config/waterhole/forum.php` under
`report_reasons`.

## Approval

Channels can require approval for new posts and/or comments. When approval is
required, content is held for moderators until it is approved from the
moderation queue.

[User Groups](./groups.md) can be configured to require approval and optionally
remove the user from the group after their content is approved.

## Edit Time Limit

Authors can be limited to editing their posts and comments within a time window
after creation. Configure this in `config/waterhole/forum.php` under
`edit_time_limit`. Moderators can always edit.

## Re-verify After Inactive Days

You can require users to re-verify their email after a long period of
inactivity. Configure this in `config/waterhole/users.php` as
`reverify_after_inactive_days`.
