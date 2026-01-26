# Table

Tables display sets of data.

Use the `.table` class to apply basic table styling. Wrap in a
`.table-container` (with `tabindex="0"` for keyboard accessibility) to enable
horizontal scrolling at smaller screen sizes.

```html render
<div class="table-container" tabindex="0">
    <table class="table">
        <thead>
            <tr>
                <th>First Name</th>
                <th>Last Name</th>
                <th>Handle</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Toby</td>
                <td>Zerner</td>
                <td>@tobyzerner</td>
            </tr>
            <tr>
                <td>Toby</td>
                <td>Zerner</td>
                <td>@tobyzerner</td>
            </tr>
            <tr>
                <td>Toby</td>
                <td>Zerner</td>
                <td>@tobyzerner</td>
            </tr>
        </tbody>
    </table>
</div>
```
