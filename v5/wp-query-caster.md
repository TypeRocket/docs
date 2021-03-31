Title: Query Caster
Description: Use the WordPress query classes and cast their results to TypeRocket models 

---

## Getting Started

Many times `WP_Query`, `WP_User_Query`, `WP_Term_Query`, and `WP_Comment_Query` are very useful over the TypeRocket ORM. These classes have many hooks and are integrated with other plugins. However, it would be nice if you could use both these WordPress core classes and the TypeRocket ORM.

The `\TypeRocket\Utility\QueryCaster` is just the tool you are looking for.

## Posts

Cast the results of a [WP_Query](https://developer.wordpress.org/reference/classes/wp_query/) to any `WPPost` model. For example, the `Post` model.

```php
$results = \TypeRocket\Utility\QueryCaster::posts(
    \App\Models\Post::class, 
    ['post_status' => 'publish']
);
```

*Note: When a model is used the `post_type` of that model is applied to the WP_Query.*

## Terms

Cast the results of a [WP_Term_Query](https://developer.wordpress.org/reference/classes/wp_term_query/) to any `WPTerm` model. For example, the `Category` model.

```php
use App\Models\Category;

$results = \TypeRocket\Utility\QueryCaster::terms(
    \App\Models\Category::class, 
    ['hide_empty' => false]
);
```

*Note: When a model is used the `taxonomy` of that model is applied to the WP_Term_Query.*

## Users

Cast the results of a [WP_User_Query](https://developer.wordpress.org/reference/classes/wp_user_query/) to any `WPUser` model. For example, the `User` model.

```php
$results = \TypeRocket\Utility\QueryCaster::users(\App\Models\User::class, [
    'role' => 'administrator',
]);
```

## Comments

Cast the results of a [WP_Comment_Query](https://developer.wordpress.org/reference/classes/wp_comment_query/) to any `WPComment` model. For example, the `Comment` model.

```php
$results = \TypeRocket\Utility\QueryCaster::comments(\App\Models\Comment::class, [
    'post_id' => 1,
]);
```