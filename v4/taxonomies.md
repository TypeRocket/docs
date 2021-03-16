Title: Taxonomies
Description: Register and create custom taxonomies  for your custom post types.

---

## About Taxonomies

WordPress taxonomies are used to create relationships between posts and custom post types. For example, "tags" are used to create a relationship between posts that share a common topic. "Categories" do the same but are hierarchical and can even share relationships within themselves.

Don't let the complexities of taxonomies scare you though. TypeRocket makes using them simple and easy so you can quickly reap the benefits of custom taxonomies.

## Adding a Taxonomy

*Note: When adding custom fields to custom taxonomies you need to have a controller and model in place for that taxonomy. See the "Forms and Fields" section for more details.*

You can create a custom taxonomy with the `tr_taxonomy()` function. Using the `tr_taxonomy()` function will take care of the basic settings for you including pluralizing labels.

Take a look at creating a `Publisher` taxonomy you can use for a "books" post type. 

```php
<?php // functions.php

tr_taxonomy('Publisher');
```

This one line of code adds the taxonomy to the admin, sets all the correct labels in the navigation and applicable places, and implements the required WordPress hooks.

### Custom plural form

In some cases, you will not want TypeRocket to manage the grammar for the taxonomies plural form.

You can set your own by supplying it as the second argument when creating the taxonomy.

```php
tr_taxonomy('Publisher', 'Publishers');
```

## Modify Existing Taxonomy

To modify an existing built-in taxonomy when using `tr_taxonomy` pass the post type ID/name as the only value.

```php
tr_taxonomy('category')->setMainForm(function() {
   $form = tr_form();
   echo $form->text('Job');
});
```

If editing any other taxonomy you need to use the `init` action and call the `register()` method. Also, you may need to set the priority of the `init` action to a higher number for overriding to work.

```php
$priority = 11;

add_action('init', function() {
    $product_cat = tr_taxonomy('product_cat');

    $product_cat->setMainForm(function() {
        $form = tr_form();
        echo $form->text('Job');
    });

    $product_cat->register();
}, $priority);
```

*Note: When overriding a taxonomy created by a plugin that plugin may have its own code that TypeRocket can not override due to how that plugin works.*

## Apply: Post Types

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

To make a hierarchical taxonomy use the `setHierarchical()` method.

```php
$pub->setHierarchical();
```

## Setting a custom slug

By default, TypeRocket will use the taxonomy ID as the slug for the URL rewrite rules. You can change the slug with the `setSlug()` method. 

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

## Forms and Fields

There is a single place to add custom content within the `<form>` element for each taxonomy. You can open up these sections with the method: `setMainForm()` for placement on the taxonomy creation and editing pages.

### Example 1: Publisher with custom field

Take a look at opening up a content section and adding a custom field. Note, when you make a custom field for taxonomy terms you must create a model and controller for TypeRocket to save the data as expected. 

You can make a controller and model quickly with the [Galaxy CLI](/docs/v4/galaxy-cli).

```shell
php galaxy make:model -c term Publisher
```

Finally, in your code.

```php
<?php
$pub = tr_taxonomy('Publisher');
$book = tr_post_type('Book')->setIcon('book');
$pub->apply($book);
$pub->setMainForm(function() {
    $form = tr_form();
    echo $form->text('Company');
});
```

![Taxonomy Custom Fields](https://typerocket.com/wp-content/uploads/2016/09/typerocket-taxonomy-fields.png)

## Arguments

There are five methods to set and get arguments. Arguments are used when the taxonomy is being registered. All arguments can be [found in the WordPress codex](https://codex.wordpress.org/Function_Reference/register_taxonomy#Arguments).

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without affecting the already set values.

```php
$args = $pub->getArguments();
$args = array_merge( $args, array( 'public' => true ) );
$pub->setArguments( $args );
$public = $pub->getArgument( 'public' );
$pub->removeArgument( 'public' );
$pub->setArgument( 'public', $public );
```