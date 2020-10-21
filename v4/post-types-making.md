Title: Post Types: Making
Description: Custom icons, fields, taxonomy, meta box – in just 20 lines of code.

---

TypeRocket 4.0 is great at building custom data structures like post types and custom fields. Take a look at what you can build with 20 lines of code.

- Custom post type
- Custom post type menu icon
- Custom image and editor fields below the title
- Custom title placeholder text
- Use title support only
- Add a custom taxonomy
- Add a meta box with a custom text field

![Stacked post type](https://l.rb.typerocket.test/wp-content/uploads/2015/07/typerocket-post-type-person.png)

## Adding a Post Type

First, [install TypeRocket](https://l.rb.typerocket.test/docs/v4/installation/). Then you can get started building out an advanced "team" [post type](https://l.rb.typerocket.test/docs/v4/post-types/) right in the `functions.php` of our theme.

```php
<?php // functions.php
tr_post_type('Person', 'Team');
```

That is it! The `tr_post_type()` function adds the post type. No hooks needed.

1. The first parameter of `tr_post_type()` is the singular name of our post type.  It will be the post types name/id and labeling.
2. The second is the plural name and is mainly for labeling.

## Changing the Post Type Registered Name

In some cases, you do not want the name WordPress registers the post type as to be the singular name. Change it by setting the object's id.

```php
$team = tr_post_type('Person', 'Team');
$team->setId('tr_team');
```

Finally, be sure you [flush the WordPress permalinks](https://l.rb.typerocket.test/flushing-permalinks-in-wordpress/).

## Adding an Icon

You can chain methods of the `PostType` object returned by the `tr_post_type()` function. Don't let the naming confuse you. As an example, lets set the icon for the post type with one of the 200+ we have in TypeRocket. (IcoMoon Free Pack, GPL)

```php
$team->setIcon('users');
```

## Overriding Settings

You can override any of the TypeRocket defaults settings for post types. The settings/args are the same as you normally use with `register_post_type()` in the WordPress API.

```php
$team->setArgument('supports', ['title'] );
```

The settings you picked for the post type gives support for just the title field. This removes the default content editor.

## Custom MetaBox

Add some custom fields to `tr_team`. You want them in a [meta box](https://l.rb.typerocket.test/docs/meta-boxes/). Add a meta box to the post type first.

```php
tr_meta_box('Team Details')->apply($team);
```

## Custom Taxonomy

Do the same with a [taxonomy](https://l.rb.typerocket.test/docs/taxonomies/).

```php
tr_taxonomy('Department')->apply($team);
```

## Custom Fields

In the meta box, with dev mode on, TypeRocket gives supplies the callback you need to echo out [form](https://l.rb.typerocket.test/docs/v4/forms/) fields.

![Typerocket meta box debug helper ](https://l.rb.typerocket.test/wp-content/uploads/2015/07/typerocket-helper-metabox.png)

Add a custom field for "Job Title".

```php
function add_meta_content_team_details() {
    $form = tr_form();
    echo $form->text('Job Title');
}
```

## Custom Title Placeholder Text

You want the title to be used for the person's name but the placeholder text says "Enter title here". You need to change the placeholder text to "Enter full name here".


```php
$team->setTitlePlaceholder( 'Enter full name here' );
```

## Custom Image and Editor

You do not want the person's photo or about field in the meta box. Add them after the title in the content area.

First the image.

```php
$team->setTitleForm(function(){
    $form = tr_form();
    echo $form->image('Photo');
});
```

### Saving to built in tables

Next the editor field for the main content.

Take the about field one step further. Using an editor field lets save the content to `post_content` in the `wp_posts` table.

```php
$team->setTitleForm(function(){
    $form = tr_form();
    echo $form->image('Photo');
    $editor = $form->editor('post_content');
    echo $editor->setLabel('About Person');
});
```

*Note: When a field uses the name of a column in the WordPress database it will be saved to it instead of being saved as meta data.*


## The Code

20 lines of code. If you want to see this on the front-end you are ready to theme with custom fields.

```php
<?php // functions.php

$team = tr_post_type('Person', 'Team');
$team->setId('tr_team');
$team->setIcon('users');
$team->setArgument('supports', ['title'] );
tr_meta_box('Team Details')->apply($team);
tr_taxonomy('Department')->apply($team);

function add_meta_content_team_details() {
    $form = tr_form();
    echo $form->text('Job Title');
}

$team->setTitlePlaceholder( 'Enter full name here' );

$team->setTitleForm( function() {
    $form = tr_form();
    echo $form->image('Photo');
    $editor = $form->editor('post_content');
    echo $editor->setLabel('About Person');
} );

```

## Templating

Now that you have a custom post type with some custom fields you are ready to start templating. Learn about templating with custom fields in the [Post Types: Theming Custom Fields Tutorial](/docs/v4/post-types-theming/).

## Security

Finally, you will want to enhance the security of the forms you have created for the post type. You can read more about this is the [Post Types: Fully Secured Tutorial](/docs/v4/post-types-secured/) 