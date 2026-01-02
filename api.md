# JSON:API

Use the Waterhole JSON:API to access forum data externally. You can extend it with your own attributes, resources, and endpoints.

The Waterhole JSON:API conforms to the [JSON:API specification](https://jsonapi.org/format/1.1/). It is powered by the [json-api-server](https://github.com/tobyzerner/json-api-server) library which makes it easy to define resource schema.

## Consuming the API

### Configuration

You can configure the JSON:API in `config/waterhole/api.php`. Here you can enable or disable the API, configure whether the API is accessible to the public, and configure the API path.

### OpenAPI Description

You can generate an OpenAPI description of your Waterhole API by running:

```bash
php artisan waterhole:openapi
```

The OpenAPI document will be saved into a `waterhole-openapi.json` file your project root.

### Authentication

The Waterhole JSON:API supports authentication using [Laravel Sanctum](https://laravel.com/docs/12.x/sanctum). If the API is configured to be public, then authentication is optional, and any Waterhole user will be able to create an API token for authentication in their account preferences. Otherwise, if the API is not public, authentication is required and only administrators can create API tokens.

Once an API token has been created, it can be used to authenticate API requests by setting it as a `Bearer` token in the `Authorization` header.

### Resources

The JSON:API is made up of "resources" identified by a `type` and an `id`.
All endpoints live under the configured API path (default `/api`).

Built-in resource types include:

- channels
- channelUsers
- comments
- groups
- pages
- posts
- postUsers
- reactionCounts
- reactionSets
- reactionTypes
- reactions
- structure
- structureHeadings
- structureLinks
- tags
- taxonomies
- users

Endpoints vary per resource. The core API is read-only (index and show), and
extensions can add write endpoints if needed. Use the OpenAPI description to
see the full list of endpoints, fields, filters, and relationships.

### Including Relationships

Use `include` to sideload related resources:

```
GET /api/posts/1?include=user,channel,tags
```

Only relationships marked as includable can be requested; invalid includes
return a JSON:API error.

### Sparse Fieldsets

To request a subset of fields for a resource type, use `fields[TYPE]`:

```
GET /api/posts/1?fields[posts]=title,bodyHtml
```

Fields marked as "sparse" are omitted unless requested. For example,
`posts.body`, `posts.bodyText`, `comments.body`, and `comments.bodyText` are
sparse by default.

### Filtering

List endpoints can be filtered with `filter[...]` parameters:

```
GET /api/posts?filter[channel]=1&filter[isLocked]=true
```

Available filters depend on the resource.

### Sorting

List endpoints can be sorted with `sort`. Prefix a field with `-` for descending
order:

```
GET /api/posts?sort=-createdAt
```

### Pagination

Paginated endpoints use offset pagination:

```
GET /api/posts?page[offset]=0&page[limit]=20
```

When supported, the response includes `meta.page.total` with the total count.

### Relationship Endpoints

Relationships can also be fetched via JSON:API relationship URLs:

```
GET /api/posts/1/comments
GET /api/posts/1/relationships/comments
```

### Errors

Errors follow JSON:API's `errors` format, and invalid query parameters are
reported with `source.parameter` when possible.

## Extending the API

### Adding Resources

```php
use Waterhole\Extend;

$this->extend(function (Extend\Api\JsonApi $api) {
    $api->resource(MyResource::class);
});
```

### Amending Resources

```php
use Tobyz\JsonApiServer\Schema\Field\Attribute;
use Waterhole\Extend;

$this->extend(function (Extend\Api\PostsResource $resource) {
    $resource->fields->add(Attribute::make('myField'), 'myField');
});
```

### Adding Routes

Register API routes directly inside the extender callback using the `Route` facade:

```php
use Illuminate\Support\Facades\Route;
use Waterhole\Extend;

$this->extend(function (Extend\Routing\ApiRoutes $routes) {
    Route::get('test');
});
```
