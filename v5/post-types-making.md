Title: Post Types: Making
Description: Custom icons, fields, taxonomy, meta box – in just 20 lines of code.

---

## Getting Started

In this TypeRocket Pro tutorial, you will learn how to make an advanced WordPress post type, with custom fields, in under 17 lines of code. Packed into those few 17 lines, you will create:

-  A custom post type 
- Set a custom menu icon
- Set custom title placeholder text
- Enable the REST API
- Set the archive page limit to infinity
- Add custom fields
- Add a meta box
- Add custom field column to post type's index table
- Remove the default editor and replace it with [Redactor 3](https://imperavi.com/redactor/).
- Add a custom taxonomy

![Stacked pro post type](https://typerocket.com/wp-content/uploads/2020/01/docs-v1-post-type-example.png)

Before starting [install TypeRocket Pro](https://typerocket.com/docs/v5/plugin-install/).

Here is all the code it takes to create the listed features. We will break the code down into its parts and explain how each part works next.

```php
// Setup
$team = tr_post_type('Person', 'Team');
$team->setIcon('dashicons-groups');
$team->setSupports(['title']);
$team->setTitlePlaceholder( 'Enter full name here' );
$team->setRest();
$team->setArchivePostsPerPage(-1);

// Custom Fields
$team->setTitleForm( function() {
    $form = tr_form();
    echo $form->image('Photo');
    echo $form->editor('post_content')->setLabel('About Person');
});

tr_meta_box('Team Details')->apply($team)->setCallback(function() {
    $form = tr_form();
    echo $form->text('Job Title');
});

$team->addColumn('Job Title');

// Taxonomy
tr_taxonomy('Department')->apply($team);
```

## Adding a Post Type

Adding an advanced post type is easy with TypeRocker Pro. In the `functions.php` of your active theme, add the following code.

```php
<?php // wp-content/themes/your-theme/functions.php
tr_post_type('Person', 'Team');
```

That is it! The `tr_post_type()` function adds the post type. No WordPress `add_action()` hooks needed.

1. The first parameter of `tr_post_type()` is the singular name of our post type.  It will be the post types name/id and labeling.
2. The second parameter is the plural name and will be used for the base slug and labeling.

[Flush the WordPress permalinks](https://typerocket.com/flushing-permalinks-in-wordpress/) each time you change the ID and slug of your post types. Flush them now.

### Adding an Icon

Set the icon for the post type with one of the 200+ [WordPress Dashicons](https://developer.wordpress.org/resource/dashicons/).

```php
$team->setIcon('dashicons-groups');
```

### Choose Your Supported Features

Next, choose which features your post type should support... the `title`, `editor`, or `thumbnail`.

For this example, we will only support the `title`. This will allow us to use Redactor for the editor field instead of TinyMCE later on. 

```php
$team->setSupports(['title']);
```

### Set Custom Title Placeholder Text

For this post type, we want the title field to be used for the person's name but the placeholder text says "Enter title here". To make for better user experience, change the placeholder text to "Enter full name here".

```php
$team->setTitlePlaceholder( 'Enter full name here' );
```

### Enable The REST API

To able REST API support for your post type, use the `setRest()` method. The plural form for your post type will be the REST API endpoint slug. 

```php
$team->setRest();
```

### Set The Archive Page Limit

Next, set the post type's archive page limit to `-1` or any positive number you like. This will modify the main query for the archive page without needing to add your own `pre_get_posts` hook. 

```php
$team->setArchivePostsPerPage(-1);
```

You can also customize the query more using the `setArchiveQueryKey()` method. For example, you can do the same as the above using this more generic method.

```php
$team->setArchiveQueryKey('post_per_page', -1);
```

## Custom Fields: Inline

Next, we add an image field, editor field, and text field to the post type. To add custom fields after the main WordPress title field in the content area use: `setTitleForm()`.

### Image Field

First, add the photo or image field with the name "Photo". The fields name will tell WordPress how its input should be stored:

```php
$team->setTitleForm(function(){
    $form = tr_form();
    echo $form->image('Photo');
});
```

When this field is saved it will add an entry into the `wp_postmeta` with the meta key of `photo` and the value will be the ID of the image attachment. 

Notice, when a field is saved to the database, its name is converted to lowercase and snake case. For example, if the field name is "Persons Photo" with the database key will be `persons_photo`.

### Editor & Overrideing Table Fields

Next, add the editor field for the main content but... we will take the editor field one step further than normal and save the editor field's input to the `post_content` column of the `wp_posts` table instead of `wp_postmeta`.

```php
$team->setTitleForm( function() {
    $form = tr_form();
    echo $form->image('Photo');
   
    // Override the built-in field and add a perttier label
    echo $form->editor('post_content')->setLabel('About Person');
});
```

When a field has the name of a column in its model's database table, that field's input will be saved to the corresponding table's column instead of being saved as metadata. This is true for user, term, and comment fields.

## Custom Fields: In a Meta Box

First, create a [meta box](https://typerocket.com/docs/v5/meta-boxes/) and apply the post type to that meta box.

```php
$meta = tr_meta_box('Team Details')->apply($team);
```

Next, add a custom field for "Job Title".

```php
$meta->setCallback(function() {
    $form = tr_form();
    echo $form->text('Job Title');
});
```

## Post Type Field Table Column

Now that you have the "Job Title" field, you can add it to the post type's admin index table.

```php
$team->addColumn('Job Title');
```

![docs-v1-post-type-table-example](https://typerocket.com/wp-content/uploads/2020/01/docs-v1-post-type-table-example.png)

## Custom Taxonomy

Finally, you can add a custom taxonomy called "Department" and apply the post type to that new taxonomy.

```php
tr_taxonomy('Department')->apply($team);
```

And, there you have it, a supercharged post type with no more than 17 lines of code.

## Templating

Now that you have a custom post type with some custom fields, you are ready to start templating. Learn about templating with custom fields in the [Post Types: Theming Custom Fields Tutorial](/docs/v5/post-types-theming/).

## Security

Finally, you will want to enhance the security of the forms you have created for the post type. You can read more about this is the [Post Types: Fully Secured Tutorial](/docs/v5/post-types-securing/) 