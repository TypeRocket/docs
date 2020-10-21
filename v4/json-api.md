Title: JSON API
Description: The TypeRocket JSON API

---

TypeRocket comes with a JSON API that can be used to `create`, `update`, and `destroy` resources. You can access this API using the following URI scheme:

```
/tr_json_api/v1/{resource_id}/{item_id}
```

Also, the `Form` object has quick access to the JSON API via the `useJson()` method.

```php
tr_form('book', 'create')->useJson();
```

The JSON API passes through the [Kernel](https://l.rb.typerocket.test/docs/v4/kernel/) [middleware](https://l.rb.typerocket.test/docs/v4/middleware/) for enhanced security. Modifications made to the Kernel will impact the JSON API.

If you encouter issues keep in mind the TypeRocket JSON API requires strict adherence to the controller action naming scheme: `showRest`, `create`, `update`, and `destroy`.

## Register Resources

By default, TypeRocket registers its custom post types and taxonomies and the default `post` and `page` post types to the JSON API automatically. However, if you have custom post types or taxonomies registered outside of TypeRocket and you want to use the REST API you register those resources.

To register a post type need to do so using `Registry::addPostTypeResource()` or `Registry::addTaxonomyResource()`.

### Post Types

To register a post type use the `Registry::addPostTypeResource()` method.

```php
Registry::addPostTypeResource('book', [
        'book', // singular
        'books', // plural
        '\App\Models\Book', // Model class
        '\App\Controllers\BookController', // Controller class
]);
```

### Taxonomies

To register a taxonomy use the `Registry::addTaxonomyResource()` method.

```php
Registry::addTaxonomyResource('publisher', [
        'publisher', // singular
        'publishers', // plural
        '\App\Models\Publisher', // Model class
        '\App\Controllers\PublisherController', // Controller class
]);
```

### Custom Resources

If you want to access your [custom resource](https://l.rb.typerocket.test/docs/v4/custom-resources/) using the TypeRocket JSON API register is using the `Registry::addCustomResource()` method.

```php
Registry::addCustomResource('test', [
        'test', // singular
        'tests', // plural
        '\App\Models\Test', // Model class
        '\App\Controllers\TestController', // Controller class
]);
```