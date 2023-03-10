# Structure
Set up the structure of your community with customizable channels, pages, and links.

Your community structure will help users understand what your community is about, navigate it, and categorize and find discussions.

You can manage your community structure in the **Structure** section of the admin panel. The structure you define will be displayed in the sidebar on your community's homepage, allowing users to easily find what they are looking for.

## Channels
Channels are the areas in your community where discussions take place. On a new installation, Waterhole automatically creates a few Channels to help you get started:

| Channel       | Description                                              |
| ------------- | -------------------------------------------------------- |
| 📣 **Announcements** | News and other updates from the team.                    |
| 👋 **Introductions** | New to the community? Introduce yourself!                |
| ❓ **Support**       | Get help setting up, using, and customising our product. |
| 💡 **Ideas**         | Have an idea? We want to hear it!                        |
| 🔒 **Staff Only**    | A private channel for staff discussion.                                                         |

Feel free to keep these, change them, or delete them – you're the boss! In general, it's best to start simple with only a few channels so your users don't get overwhelmed.

### Channel Options
When creating a channel, you will need to choose a name, URL slug, icon, and a short description. There are also a number of additional options available:

- **Posting Instructions** allow you to specify more detailed information to be displayed to users when they are creating a post in the channel. This is a good place to remind users about rules and guidelines for posting in the channel – such as information that is required in a support channel.

* **Ignored by Default** makes the channel opt-in. Its posts will be hidden from the community homepage, so they can only be seen by navigating to the channel. However, users will still be able to individually follow the channel to include its posts on their homepage. This option is useful for more niche channels that only a small number of users will be interested in.

* **Default Layout** allows you to choose whether posts display in a list, or as expanded cards, when viewing the channel. This is useful for particular channels that are more post-oriented than discussion-oriented – for example, a blog channel.

* **Filter Options** allows you to override the [global filter options](./filters.md) for the channel. The first one will be used as the default.

You can also configure permissions to restrict which [User Groups](./groups.md) can view, post, comment, and moderate (edit and delete content) in the channel.

## Pages
Waterhole allows you to set up basic content pages within your community structure. This is useful for things like Community Guidelines, Terms & Conditions, or a Privacy Policy.

It's a good idea to have a page which lays out what's expected of participants in your community, and gives some pointers about how to navigate and use the community features. For this reason, on a new installation, Waterhole automatically sets up a 📖 **Community Guide** page with a few sensible guidelines to help you get started. You might like to tweak this or start from scratch.

In addition to the content, pages need to be given a name, URL slug, and icon. You can also configure permissions to restrict which [User Groups](./groups.md) can view the page.

## Links
Links allow you to add navigation items that link to external URLs. This could be a link to your homepage, to your social profiles, or to any other website you'd like to be easily accessible. It could even be a link to a specific post in your community!

Like channels and pages, links can have a name, an icon, and permissions to restrict which user groups can view them.

## Headings
Headings are a way to organize your channels, pages, and links. You may not need to use any headings if your structure is nice and simple, but they're there if you do need them.

You can also create a heading with an empty name to just add a blank space between your items.

Visibility permissions on headings aren't necessary, because they automatically get hidden if there are no non-heading items beneath them.

## Unlisted Items
On the Structure page, below the main list of nodes, there is a spot for "unlisted" nodes. Unlisted channels and pages will not be shown in the navigation sidebar, and all of the content and posts inside of them will be hidden from search engines. However, they will still be accessible directly by their URL slug.

> **Warning:**  
> Just because an item is unlisted, it doesn't mean it's not accessible. If an unlisted item has open permissions and someone obtains the URL, they will be able to see it. Always ensure your permissions are configured correctly.