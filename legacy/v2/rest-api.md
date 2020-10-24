Title: REST API
Description: Update and create resources like users, comments, options or post types.

---

## About the REST API

If you are a developer the REST API is for you. If you are a designer don't worry. The REST API is designed to work out of the box so you don't need to know all the inner workings.

The TypeRocket REST API is located at `http://{your-domain}/typerocket_rest_api/v1/`. Out of the box the REST API supports two methods `PUT` for updating and `POST` for creating. `DELETE` and `GET` are supported but are not required implementations if you are extending the REST API.

To start using the REST API you need to flush the WordPress rewrite rules. You can do this in the admin by clicking `Settings > Permalinks > Save Changes`.

## Resources

TypeRocket will automatically setup resources for posts, pages, options, users and comments.

If you are using custom post types they will default to the `posts` resource unless you register the resource using `Registry::addPostTypeResource()`. If you are using TypeRocket to register or create a custom post type your resource will be automatically registered. When TypeRocket registers the resource for you it will use the plural form of the post types name.

### Register a post type resource

To register a post type resource manually `Registry::addPostTypeResource()` takes two arguments.

1. A `string` for the post type ID / name.
2. A `string` for the resource name with only lowercase letters.

### Example 1: book post type

If you have a post type that was not created using TypeRocket with the ID of `book` the best resource name would be `books`. In most cases the resource name should be the plural form of the post type. 

```php
Registry::addPostTypeResource('book', 'books');
```

### Example 2: irregular post types

In some cases the post type might have a strange name like `wp_help_post`. In cases like this, since you need to use only lowercase letters, you might use the resource name `helpers`.

```php
Registry::addPostTypeResource('wp_help_post', 'helpers');
```

## Request and Response

The TypeRocket REST API is designed to work specifically with TypeRocket forms. When a `Form` is submitted using the REST API via AJAX, like with the theme options plugin, a custom `Request` and `Response` object is created and sent through the TypeRocket `Kernel`.

The `Kernel` sends the `Request` and `Response` through appropriate middleware. At the center of the middleware if the controller for the resource. For the options resource the controller is `OptionsController`.

![REST API](https://typerocket.com/wp-content/uploads/2015/08/docs-rest-api.png)