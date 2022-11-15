Title: Taxonomies
Description: Register and create custom taxonomies  for your custom post types.

---

## About Taxonomies

WordPress taxonomies are used to create relationships between posts and custom post types. For example, "tags" are used to create a relationship between posts that share a common topic. "Categories" do the same but are hierarchical and can even share relationships within themselves.

Don't let the complexities of taxonomies scare you.

## Adding a Taxonomy

! **Note**: When adding custom fields to custom taxonomies, you need to have a controller and model in place for that taxonomy. See the "Forms and Fields" section for more details.

You can create a custom taxonomy with the `tr_taxonomy()` function. Using the `tr_taxonomy()` function will take care of the basic settings for you, including pluralizing labels.

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

If editing any other taxonomy, you need to use the `init` action and call the `register()` method. Also, you may need to set the priority of the `init` action to a higher number for overriding to work.

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

! **Note**: When overriding a taxonomy created by a plugin that plugin may have its own code that TypeRocket can not override due to how that plugin works.

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

## Show Quick Edit

Whether to show the taxonomy in the quick/bulk edit panel.

```php
$pub->showQuickEdit(true);
```

## Show Post Type Admin Column

Whether to allow automatic creation of taxonomy columns on associated post-types table.

```php
$pub->showPostTypeAdminColumn(true);
```

## Set Rest

Add taxonomy to the REST API and set the base path.

```php
$pub->setRest('publishers');
```


## Setting a custom slug

By default, TypeRocket will use the taxonomy ID as the slug for the URL rewrite rules. You can change the slug with the `setSlug()` method. 

```php
$pub->setSlug('publishers');
```

### Slug With Front

Sometimes you will not want your taxonomy slug structure be prepended to the front base. For example, if your main permalink structure is `/blog/%postname%/` then your links will be `/blog/publishers/%postname%/` by default because `with_font` is `true` by default. To make taxonomy slug stop prepending to the front base URL set the `with_front` setting to `false`; the result will be `/publishers/%postname%/` instead of `/blog/publishers/%postname%/`.

```php
$withFront = false;
$pub->setSlug('publishers', $withFront);
```

Or, you can simply disable `with_front`.

```php
$pub->disableSlugWithFront();
```

## Show Or Hide Admin & Frontend

Sometimes you don't want a taxonomy to have an archives page. You can use the `setAdminOnly()` method to keep a taxonomy out of the front-end.

Take a look at making a new post type that is admin only.

```php
$pub->setAdminOnly();
```

Or, inversely you can hide the taxonomy from the admin.

```php
$pub->hideAdmin();
```

To hide the taxonomy from the front-end.

```php
$pub->hideFrontend();
```


## Setting and Getting the ID

The ID is used to specify the name that WordPress associates with your taxonomy. It is also registered under the ID. When you register "Publisher" with TypeRocket, the id is set to "publisher".

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

Take a look at opening up a content section and adding a custom field. Note, when you make a custom field for taxonomy terms, you must create a model and controller for TypeRocket to save the data as expected. 

You can make a controller and model quickly with the [Galaxy CLI](/docs/v6/galaxy-cli).

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

## Custom Labels

To customize taxonomy labels you can use the shortcut setting `labeled`. This setting makes setting labels for translation easier for plugins to detect and for you to set.

```php
tr_taxonomy('Publisher', 'Publishers', [
    'labeled' => [
        __('Publisher'),
        __('Publishers'),
        false, // Keep capitalization
    ]
]);

tr_taxonomy('CTX', 'CTXs', [
    'labeled' => [
        __('CTX'),
        __('CTXs'),
        true, // Keep capitalization
    ]
]);
```

### Complete Override

You can also completely override the labels using the `setLabels()` method:

```php
$upperPlural = 'Publishers';
$upperSingular = 'Publisher';
$lowerSingular = 'publisher';
$lowerPlural = 'publishers';

tr_post_type('Book', 'Books')->setLabels([
    'add_new_item'               => sprintf( _x( 'Add New %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'add_or_remove_items'        => sprintf( _x( 'Add or remove %s', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'all_items'                  => sprintf( _x( 'All %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperPlural),
    'back_to_items'              => sprintf( _x( '← Back to %s', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'choose_from_most_used'      => sprintf( _x( 'Choose from the most used %s', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'edit_item'                  => sprintf( _x( 'Edit %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'name'                       => sprintf( _x( '%s', 'taxonomy:publisher:taxonomy general name', 'your-custom-domain' ), $upperPlural),
    'menu_name'                  => sprintf( _x( '%s', 'taxonomy:publisher:admin menu', 'your-custom-domain' ), $upperPlural),
    'new_item_name'              => sprintf( _x( 'New %s Name', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'no_terms'                   => sprintf( _x( 'No %s', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'not_found'                  => sprintf( _x( 'No %s found.', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'parent_item'                => sprintf( _x( 'Parent %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'parent_item_colon'          => sprintf( _x( 'Parent %s:', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'popular_items'              => sprintf( _x( 'Popular %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperPlural),
    'search_items'               => sprintf( _x( 'Search %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperPlural),
    'separate_items_with_commas' => sprintf( _x( 'Separate %s with commas', 'taxonomy:publisher', 'your-custom-domain' ), $lowerPlural),
    'singular_name'              => sprintf( _x( '%s', 'taxonomy:publisher:taxonomy singular name', 'your-custom-domain' ), $upperSingular),
    'update_item'                => sprintf( _x( 'Update %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
    'view_item'                  => sprintf( _x( 'View %s', 'taxonomy:publisher', 'your-custom-domain' ), $upperSingular),
], 'Book');
```

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

## Custom Capabilities

If you would like to set custom capabilities for a taxonomy, use the `customCapabilities()` method. This method will replace the taxonomy's capability settings with the singular and plural taxonomy name.

```php
$pub = tr_taxonomy('Publisher');
$pub->customCapabilities();

// Capabilities will become:
//
// manage_terms -> manage_publishers
// edit_terms -> edit_publishers
// delete_terms -> delete_publishers
// assign_terms -> assign_publishers
```

Once a taxonomy has custom capabilities, you will need to apply those capabilities to a role. Further, roles should only be updated on plugin or theme (de)activation.

```php
// After theme/plugin (de)activation
$caps = tr_roles()->getTaxonomyCapabilities('publisher', 'publishers');
tr_roles()->updateRolesCapabilities('administrator', $caps);
```

## Set Handler & Controller

By default, the controller for a taxonomy is automatically set to one using its shared name in the `App` namespace. To set a custom controller for a taxonomy use `setHandler()`.

```php
$prodType = tr_taxonomy('Product Type');
$prodType->setHandler(\MyCustomNamspace\Controllers\ProductTypeController::class);
```

## Set Model Class

By default, the model class for a taxonomy is automatically set to one using its shared name in the `App` namespace. To set a custom model class for a taxonomy use `setModelClass()`. When setting the model a `tr_form()` will locate that model for the taxonomies custom fields.

```php
$prodType = tr_taxonomy('Product Type');
$prodType->setModelClass(\MyCustomNamspace\Models\ProductType::class);
```