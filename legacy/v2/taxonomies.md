Title: Taxonomies
Description: Register and create custom taxonomies  for your custom post types.

---

WordPress taxonomies are used to create relationships between posts and custom post types. For example, "tags" are used to created a relationship between posts that share a common topic. "Categories" do the same but are hierarchical and can even share relationships within themselves.

Don't let the complexities of taxonomies scare you though. TypeRocket makes using them simple and easy so you can quickly reap the benefits of custom taxonomies.

## Creating a Taxonomy

You can create a custom taxonomy with the `tr_taxonomy()` function. Using the `tr_taxonomy()` function will take care of the basic settings for you including pluralizing labels.

Take a look at creating a `Publisher` taxonomy you can use for a "books" post type. 

```php
<?php // functions.php

tr_taxonomy('Publisher');
```

This one line of code adds the taxonomy to the admin, sets all the correct labels in the navigation and applicable places, and implements the required WordPress hooks.

### Custom plural form

In some cases you will not want TypeRocket to manage the grammar for the taxonomies plural form.

You can set your own by supplying it as the second argument when creating the taxonomy.

```php
tr_taxonomy('Publisher', 'Publishers');
```

## Adding to a post type

To add a custom taxonomy to a post type you can use the `addPostType()` method or the `apply()` method. 

### Example 1: using addPostType()

```php
<?php // functions.php

$pub = tr_taxonomy('Publisher');
$pub->addPostType('book');
``` 

### Example 2: Using apply()

```php
<?php // functions.php

$pub = tr_taxonomy('Publisher');
$book = tr_post_type('Book');
$pub->apply($book);
```

## Hierarchical

To make a taxonomy hierarchical use the `setHierarchical()` method.

```php
$pub->setHierarchical();
```

## Setting a custom slug

By default TypeRocket will use the taxonomy ID as the slug for the URL rewrite rules. You can change the slug with the `setSlug()` method. 

```php
$pub->setSlug('publishers');
```

## Setting and Getting the ID

The ID is used to specify the name that WordPress associates with your taxonomy. It is also registered under the ID. When you register "Publisher" with TypeRocket the id is set to "publisher".

You can change the ID using the `setId()` method.

```php
$pub->setId('book_publisher');
```

Get the ID

```php
$pubId = $pub->getId();
```

## Arguments

There are 5 methods for dealing with arguments. Arguments are used when the taxonomy is being registered. All arguments can be [found in the WordPress codex](https://codex.wordpress.org/Function_Reference/register_taxonomy#Arguments).

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without effecting the already set values.

```php
$args = $pub->getArguments();
$args = array_merge( $args, array( 'public' => true ) );
$pub->setArguments( $args );
$public = $pub->getArgument( 'public' );
$pub->removeArgument( 'public' );
$pub->setArgument( 'public', $public );
```