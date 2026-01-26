# Placeholder

A placeholder informs the user that some content is unavailable, either because
no data exists or something went wrong loading it.

Use the `.placeholder` class on the container, which centers and adds
appropriate spacing to the children. Then add any of an icon, heading,
description and call-to-action. The icon should use the `.placeholder__icon`
class to give it a larger size.

```blade render
<div class="placeholder">
    @icon('tabler-star', ['class' => 'placeholder__icon'])
    <h4>No Stars</h4>
    <p>Add a star to get star-ted.</p>
    <button class="btn">Add Star</button>
</div>
```
