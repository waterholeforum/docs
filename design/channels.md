# Channels
Various components are included for working with channels.

## Channel Label
A channel label displays a channel's icon and name, optionally as a link to the channel if the `link` attribute is specified.

```blade render
<x-waterhole::channel-label :channel="$channel" link/>
```

## Channel Picker
The channel picker component presents a selectable list of channels. Selecting a channel will submit the form – so the form's controller must be able to distinguish between this and submission via the main submit button.

```blade render
<x-waterhole::channel-picker
  name="channel_id"
  :value="old('channel_id')"
  :exclude="[1, 2]"
/>
```
