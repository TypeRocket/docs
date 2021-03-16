Title: Theme Options
Description: Customize the global elements of your design with theme options.

---

TypeRocket 3.0 makes adding theme options for WordPress simple. Add any number of custom fields you want.

![typerocket-theme-options](https://typerocket.com/wp-content/uploads/2015/08/typerocket-theme-options.png)

## Enabling Theme Options

TypeRocket works as a theme options framework for WordPress by providing the custom fields you need to customize your themes. To get started all you need to do is enable the `theme-options` plugin in the [TypeRocket configuration](/docs/v3/configuration/) in the file `config/app.php`.

```php
'plugins' => [
    'seo',
    'dev',
    'theme-options', // theme options plugin
    'builder',
],
```

## Your options, your ideas

Theme options are commonly used to manage the global elements of a WordPress theme's design. These elements could be as simple as the copyright information at the bottom of every page; to API keys for Google Maps or Facebook Sharing.

You can use any of the fields that come with the [forms api](/docs/v3/forms/). This means you can have 18+ types of fields.

1. Text inputs
2. Passwords
3. Hidden inputs
4. Submit buttons
5. Textareas
6. Editors
7. Radio buttons
8. Checkboxes
9. Select menus
10. WordPress Editor
11. Color pickers
12. Date Pickers
13. Images
14. Files
15. Galleries
16. Items lists
17. [Matrix Fields](/docs/v3/matrix-field/)
18. [Builder Fields](/docs/v3/builder-field/)
19. [Repeaters](/docs/v3/repeater-field/)
20. Search fields

## Going custom

Out of the box, the theme options plugin comes with a file called `admin.php`.

1. Duplicate this file.
2. Place the new file in the root of you custom theme's directory.
3. Rename the new file to `theme-options.php`.

*If you want to start from scratch delete the code inside the file.*

Next, you need to let the theme options plugin know that you are going to use `theme-options.php` as the new admin page.

### Configuring the custom page 

In your theme's `functions.php` file add a filter for `tr_theme_options_page`. This will replace `admin.php` with `theme-options.php`.  

```php
// functions.php
add_filter('tr_theme_options_page', function() {
    return get_template_directory() . '/theme-options.php';
});
```

### Configuring the theme name

To wrap up configuration, change the theme options name with the filter `tr_theme_options_name`.

```php
// functions.php
add_filter('tr_theme_options_name', function() {
    return 'my_theme_options';
});
```

## Adding custom fields

Now, let us start with the bare minimum. Use the code below as the contents of the `theme-options.php` file.

```php
<h1>Theme Options</h1>

<?php
$form = tr_form();
$form->useJson();
$form->setGroup( $this->getName() );
?>

<div class="typerocket-container">
    <?php
    echo $form->open();

    // about
    $company = $form->text('Name');
    $company .= $form->image('Logo');
    $company .= $form->textarea('About');

    // save
    $save = $form->submit( 'Save' );

    // layout
    tr_tabs()
    ->setSidebar( $save )
    ->addTab( 'Company', $company )
    ->render( 'box' );
    echo $form->close();
    ?>
</div>
```

If you have debug mode enabled you will see the export/import feature at the bottom of the page.  Otherwise, the code hints and export/import will be removed.


## Fields into your templates

With debug mode enabled you can copy the code hints into any theme template file.

Finally, you can get the basic field types into the templates: Text, Image, and Textarea.

### Text fields

For text fields, a using `echo` will work just fine. However, don't forget to sanitize all user entered data on output.

```php
// footer.php
echo tr_options_field('my_theme_options.name');
```

### Image fields

The image attachment ID is saved for image fields. Using a function like [wp_get_attachment_image()](https://codex.wordpress.org/Function_Reference/wp_get_attachment_image) will get the `<img />` tag.

```php
// header.php
$imgId = tr_options_field('my_theme_options.logo')
echo wp_get_attachment_image($imgId, 'full');
```

### Textarea fields

For a textarea you can use `nl2br()` or `wpautop()`. Because `wpautop()` adds `<p>` tags over `<br>` tags in most cases you should use that.

```php
// sidebar.php
$about = tr_options_field('my_theme_options.about');
echo wpautop($about);
```

### Advanced fields

If you decide to use advanced fields like repeaters and galleries you can use `var_dump()` to inspect the fields.

## Adding tabs and fields

Now that you have the most basic setup you can start adding more tabs and fields.

- [Tabs API](/docs/v3/layout-tabs/)
- [Forms API](/docs/v3/forms/) (fields are near the bottom)
- [Fields API](/docs/v3/fields/)