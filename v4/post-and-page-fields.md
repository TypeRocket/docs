Title: Post and Page Fields
Description: Adding custom fields within a meta box to WordPress posts and pages.

---

Custom fields are a necessity when your client needs to edit their own content. Maybe your client wants to have a unique banner image for their blog posts and a subheading under the title. Fields are exactly what they need to edit this custom content.

![Screenshot, custom WordPress post fields](https://l.rb.typerocket.test/wp-content/uploads/2015/08/typerocket-post-custom-fields.png)

## Adding custom fields to posts

With just a few lines of code, you can add the banner and subheading fields to the WordPress `post` post type using TypeRocket. All you need to do is create a [meta box](https://l.rb.typerocket.test/docs/meta-boxes/) and add it to the `post` screen. Then you can define a content callback and create the fields you need using the [forms API](https://l.rb.typerocket.test/docs/forms/).

Here you can add everything you need in your theme's `functions.php` file.

```php
<?php // functions.php
include('typerocket/init.php');

$boxPosts = tr_meta_box('Post Details');
$boxPosts->addScreen('post');
$boxPosts->setCallback(function(){
    $form = tr_form();
    echo $form->image('Banner Image');
    echo $form->text('Subheading');
});
```

## Adding custom fields to pages

If you want to add custom fields to a page you should use the same approach. All you need to do is create a new meta box and add the screen called `page`.

```php
$boxPages = tr_meta_box('Page Details');
$boxPages->addScreen('page'); // updated
$boxPages->setCallback(function(){
    $form = tr_form();
    echo $form->image('Banner Image');
    echo $form->text('Subheading');
});
```

## Modify a Post Type

You can also add fields to posts and pages without using a meta box. To do that use the `tr_post_type()` function and the `setEditorForm()` API. Taking this approach also allows you to use other TypeRocket APIs like `setIcon()` and `setTitlePlaceholder()`:

```php
tr_post_type('post')
    ->setIcon('book')
    ->setTitlePlaceholder('Enter Article Title')
    ->setEditorForm(function() {
        $form = tr_form();
        echo $form->text('Name');
    });
```