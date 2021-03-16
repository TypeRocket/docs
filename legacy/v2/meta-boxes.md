Title: Meta Boxes
Description: Making and adding meta boxes to post types,  the dashboard or comments.

---

Meta boxes are the movable boxes you see on the dashboard or when editing a post. If you have the TypeRocket SEO plugin enabled you will see that it creates a meta box for you to add extra information about your content.

![Meta box](https://typerocket.com/wp-content/uploads/2015/07/docs-metabox.png)

## Creating a WordPress meta box

To create a meta box with TypeRocket use the `tr_meta_box()` function.

When you create a meta box be sure you are not using any of the names used by [post type supports](https://codex.wordpress.org/Function_Reference/post_type_supports#Parameters).

```php
<?php // functions.php

$box = tr_meta_box('Author Details');
```

## Dashboard

To add a meta box to the dashboard use the `addScreen()` method on the newly created meta box.

```php
$box->addScreen('dashboard');
```

## Comments

To add a meta box to a comment use `addScreen()` as well.

```php
$box->addScreen('comment');
```

## Post types

To add a meta box to a post type you can use `addScreen()`, `addPostType()` or the `apply()` method.

### Example 1: using addScreen() and addPostType()

If you choose to use add screen pass in the post type's name/ID.

Take a look at adding a meta box to both a custom post type created by TypeRocket and the WordPress posts post type.

```php
$book = tr_post_type('Book');
$box = tr_meta_box('Author Details');
$box->addPostType( $book->getId() );
$box->addScreen( 'post' );
```

### Example 2: using apply()

If you use `apply()` pass the TypeRocket post type object.

```php
$book = tr_post_type('Book');
$box = tr_meta_box('Author Details');
$box->apply( $book );
```

## Debug mode

Once you have added a meta box to a screen or post type you can start adding content to it. If you have debug mode turned on TypeRocket will tell what function will be used by the meta box for the content.

![meta box debug](https://typerocket.com/wp-content/uploads/2015/07/docs-meta-box-debug-typerocket.png)

## Adding content

To add content to a meta box you can use the function debug mode gives you or you can specify your own callback.

To specify your own callback use the `setCallback()` method.

### Example 1: using your own callback

```php
$box = tr_meta_box('Author Details');

$box->setCallback(function() {
    echo '<p>My callback.</p>';
});
```

### Example 2: Using debug function

```php
tr_meta_box('Author Details');

function add_meta_content_author_details() {
    echo '<p>My callback.</p>';
}
```

## Label

If you want to manually set the label used by the meta box you need to use the `setLabel()` method. You can also get the label with `getLabel()`.

```php
$box->setLabel('Book Author Details');
```

## Setting and Getting the ID

When instance a meta box TypeRocket automatically creates the ID for you.

The ID is used to specify the name that WordPress associates with your meta box. It is also registered under the ID. When you register "Author Details" with TypeRocket the id is set to "author_details".

You can change the ID using the `setId()` method.

```php
$box->setId('author_details_box');
```

Get the ID

```php
$boxId = $box->getId();
```

## Arguments

There are 5 methods for dealing with arguments. Arguments the extra data you can use in the callback by accessing the meta box object passed into it.

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without effecting the already set values.

```php
$args = $box->getArguments();
$args = array_merge( $args, array( \'yourArg\' => true ) );
$box->setArguments( $args );
$public = $box->getArgument( \'yourArg\' );
$box->removeArgument( \'yourArg\' );
$box->setArgument( \'yourArg\', $public );
```

## Priority

To set the meta box priority use the `setPriority()` method. You can also get the priority with `getPriority()`.

There are four options for priority 'high', 'core', 'default' or 'low'.

## Context

To set the context or the meta box use `setContext()`. You can get the context with `getContext()`.

There are three options for the context 'normal', 'advanced', or 'side'.