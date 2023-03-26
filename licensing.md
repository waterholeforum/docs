# Licensing

Waterhole is free to use in development environments, but you'll need to buy a license to use it in production.

## Development

You can run Waterhole in a development environment for free, without any limitations. This includes domains with any of the following properties:

-   A single segment, like `localhost`
-   A local TLD, like `.local`, `.localhost`, `.test`, or `.example`.

## Production

When you're ready to launch your community on a public domain:

1. [Purchase a license](https://waterhole.dev/pricing) and specify which domain you will be using it on.
2. You'll be provided with a unique site key – add this to your Waterhole `.env` configuration:

```ini
WATERHOLE_SITE_KEY=your-site-key
```

Each license entitles you to run **one** production installation. You can change the domain associated with a license at any time on [waterhole.dev](https://waterhole.dev).

## License Validation

In production, Waterhole pings a license validation service hourly. This service collects the license key, public domain info (domain name, IP address, etc.) and version numbers so we can validate them against your account.

If your license is invalid, Waterhole will show an admin notification telling you what's gone wrong.

Tampering with the outgoing API call will cause Waterhole to consider your license invalid. If that happens, you'll need to open a [support request](https://waterhole.dev/support) to reinstate your license.
