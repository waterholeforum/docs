# SEO

Waterhole includes basic SEO metadata and external link handling that you can
customize via configuration.

Waterhole includes a set of sensible defaults so your forum pages look good in
search results and when shared:

- Page titles with your forum name appended.
- Meta descriptions (auto-derived when possible, with a configurable fallback).
- OpenGraph tags for title, description, URL, site name, and image (using a post's first image).
- Twitter card tags for rich social previews.
- `robots` handling for pages marked as noindex.
- JSON-LD structured data based on the page type (website, article, profile).
- External links marked with `rel="nofollow ugc"` by default.

## Configuration

Global defaults live in `config/waterhole/seo.php`:

- `default_description` for pages without a specific description.
- `default_og_image` for social previews.

### External Link Rel Attributes

By default, external links are rendered with `rel="nofollow ugc"` and
`target="_blank"`. You can adjust this behavior in `config/waterhole/seo.php`:

- `nofollow_allow` is a list of domains that should not receive nofollow.
- `nofollow_rel` is the rel attribute for external links not on the allowlist.
