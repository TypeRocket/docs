Title: REST API
Description: The TypeRocket REST API

---

## TypeRocket REST API

TypeRocket comes with a REST API that can be used to `create`, `update`, and `destroy` resources. You can access this API using the following URI scheme:

```
/tr-api/rest/{resource_id}/{item_id}
```

Also, the `Form` object has quick access to the JSON API via the `useRest()` method.

```php
tr_form('book', 'create')->useRest();
```

The JSON API passes through the kernel [middleware](https://typerocket.com/docs/v6/middleware/) for enhanced security. Modifications made to the Kernel will impact the REST API.

If you encounter issues, the REST API requires strict adherence to the controller action naming scheme: `showRest`,  `indexRest`, `create`, `update`, and `destroy`.

## Register Resources

By default, TypeRocket registers its custom post types and taxonomies and the default `post` and `page` post types to the REST API automatically. However, if you have custom post types or taxonomies registered outside of TypeRocket and you want to use the REST API, you register those resources.

To register a post type need to do so using `Registry::addPostTypeResource()` or `Registry::addTaxonomyResource()`.

### Post Types

To register a post type use the `Registry::addPostTypeResource()` method.

```php
\TypeRocket\Register\Registry::addPostTypeResource('book', [
	'singular' => 'book',
	'plural' => 'books',
	'controller' => '\App\Controllers\BookController',
]);
```

### Taxonomies

To register a taxonomy use the `Registry::addTaxonomyResource()` method.

```php
\TypeRocket\Register\Registry::addTaxonomyResource('publisher', [
	'singular' => 'publisher',
	'plural' => 'publishers',
	'controller' => '\App\Controllers\PublisherController',
]);
```

### Custom Resources

If you want to access your custom resource using the TypeRocket REST API register is using the `Registry::addCustomResource()` method.

```php
\TypeRocket\Register\Registry::addCustomResource('test', [
	'controller' => '\App\Controllers\TestController',
]);
```

## Middleware

The TypeRocket REST API has special rules for loading the correct middleware group. See the middleware docs.