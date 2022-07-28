Title: Removing the Editor from Post Types
Description: In some cases you don't want to use the WordPress editor.

---

The WordPress editor has gotten a lot better over the years. However, You don't always want to use it. A custom post type used for photo galleries doesn't need the editor; but how do you remove it? By using the `remove_post_type_support()` function.

## Custom Post Type

Think about the `gallery` post type. All it needs are photos. To remove the editor:

1. Open your themes `functions.php` file.
2. Pass the post type ID as the first argument for `remove_post_type_support()`.
2. Then, call the function during the `init` action and set the priority to 99.

```php
<?php // functions.php
add_action( 'init', function() {
    remove_post_type_support( 'gallery', 'editor' );
}, 99);
```

*Note: Setting the priority to 99 will make sure it is called after your post type has been registered. You can always increase the number. 99999 might work better for you.*

### Posts and Pages

If you want to remove the editor from the `post` and `page` post types do can do the same.

```php
add_action( 'init', function() {
    remove_post_type_support( 'post', 'editor' );
    remove_post_type_support( 'page', 'editor' );
}, 99);
```

### Support Title Only

If you are [registering a post type with TypeRocket](https://typerocket.com/docs/v4/post-types) and only want the main title field you can use the `setArgument()` method instead.

```php
$gallery = tr_post_type('Gallery');
$gallery->setArgument('supports', ['title'] );
```

## WordPress Supports List

WordPress has a list of features any post type can add or remove support for.

- `title` for the main title field.
- `editor` for the WordPress editor.
- `author` for the publishing author select box.
- `thumbnail` for the featured image.
- `excerpt` for the excerpt meta box.
- `trackbacks` for trackbacks if the content is linked to externally.
- `custom-fields` for the WordPress built-in custom fields meta box.
- `comments` for comments.
- `revisions` for revisions of the post type.
- `page-attributes` for template and menu order (hierarchical only).
- `post-formats` for post formats.