Title: Theme Options
Description: Customize the global elements of your design with theme options.

---

*Pro Only: This is a Pro only extension feature.*

## Enabling Theme Options

TypeRocket works as a theme options framework for WordPress by providing the custom fields you need to customize your themes. To get started all you need to do is ensure the `ThemeOptions` extension is in the [TypeRocket configuration](/docs/v5/configuration/) in the file `config/app.php`. 

## Your options, your ideas

Theme options are commonly used to manage the global elements of a WordPress theme's design. These elements could be as simple as the copyright information at the bottom of every page to API keys for Google Maps.

You can use any of the fields that come with the [forms api](/docs/v5/forms/).

## Going custom

Out of the box, the theme options plugin comes with its own view file under `app/resources/pages/extensions/theme-options.php`. You will need to tell the extensions you want to use another file.

1. Duplicate the `theme-options.php` file.
2. Place the new file in the root of your active theme's directory.

*If you want to start from scratch delete the code inside the file.*

Next, you need to let the theme options plugin know that you are going to use `theme-options.php` as the new admin page.

### Configuring the custom page 

In your theme's `functions.php` file add a filter for `tr_theme_options_controller` and add your own controller code.  

```php
// functions.php
add_filter('typerocket_theme_options_controller', function($controller) {
    return function($themeOptions) {
	     $form = tr_form()->useRest()->setGroup( $themeOptions->getName() );
	    return tr_view( __DIR__ . '/theme-options.php', ['form' => $form]);
    };
});
```

### Configuring the theme name

To wrap up configuration, change the theme options name with the filter `tr_theme_options_name`.

```php
// functions.php
add_filter('typerocket_theme_options_name', function() {
    return 'my_theme_options';
});
```

## Adding custom fields

Now, let us start with the bare minimum. Use the code below as the contents of the `theme-options.php` file.

```php
<h1>Theme Options</h1>  
<?php  
/** @var \TypeRocket\Elements\Form $form */  
echo $form->useRest()->open();
  
// About  
$about = $form->fieldset('Company', 'Details about your company.', [  
  $form->text('Company Name'),  
  $form->image('Company Logo'),  
  $form->texteditor('Company Info'),   
]); 
  
// Save  
$save = $form->submit( 'Save Changes' );  
  
// Layout  
$tabs = tr_tabs()->setFooter( $save )->layoutLeft();  
$tabs->tab('About', 'building', $about)->setDescription('Company information');  
$tabs->render();  
  
echo $form->close();  
?>
```

## Fields into your templates

With debug mode enabled, you can copy the code hints into any theme template file.

Finally, you can get the basic field types into the templates: Text, Image, and Textarea.

### Text field

For text fields, using `echo` will work just fine. However, don't forget to sanitize all user-entered data on output.

```php
// footer.php
echo tr_option_field('my_theme_options.company_name');
```

### Image field

```php
// header.php
echo tr_option_field(':img:full:my_theme_options.company_logo');
```

### Textarea field

For a textarea you can use `nl2br()` or `wpautop()`. Because `wpautop()` adds `<p>` tags over `<br>` tags in most cases you should use that.

```php
// sidebar.php
$about = tr_options_field('my_theme_options.company_info');
echo wpautop(esc_html($about));
```

Or,  for rich HTML content.

```php
echo tr_options_field('html:my_theme_options.company_info');
```

### Advanced fields

If you decide to use advanced fields like repeaters and galleries, you can use `var_dump()` to inspect the fields.

## Adding tabs and fields

Now that you have the most basic setup, you can start adding more tabs and fields.

- [Tabs API](/docs/v5/layout-tabs/)
- [Forms API](/docs/v5/forms/) (fields are near the bottom)
- [Fields API](/docs/v5/fields/)