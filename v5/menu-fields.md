Title: Add Custom Fields to Menu Items
Description: Add custom fields to menu items in WordPress 5.4+.

---

## Getting Started

!!

First things first, to enabled WordPress menus for your theme, you need to register them. If you already have menus enabled for your theme you can skip the registration process.

In your theme's `functions.php` file add the following:

```php
// functions.php
register_nav_menu('main', 'Main Menu');
```

To start adding fields to your menu items, use the TypeRocket hook `tr_menu_fields`. This hook provides access to a preconfigured TypeRocket form object that you must use to add your custom fields.

Take a look at this example, where we will add a text field named `sub_label`.
 
```php
// functions.php
add_action('typerocket_menu_fields', function($form) {  
    /** @var \TypeRocket\Elements\Form $form */  
  echo $form->text('Sub Label');  
});
```

That's it! You now have a menu item field.

*Note: Only simple fields work in menu items right now. Also, conditional fields are not supported for menu items at the moment but they will be added in a coming version.*

### Displaying the menu item field

Next, to display the menu item field, you will need to use the WordPress hook `wp_nav_menu_objects`. This hook is used to modify the HTML returned by the [wp_nav_menu()](https://developer.wordpress.org/reference/functions/wp_nav_menu/) function.

```php
// functions.php
add_filter('wp_nav_menu_objects', function ( $items, $args ) {

	foreach( $items as &$item ) {		
		if( $label = tr_field('sub_label', $item) ) {
			$label = esc_html($label);
			$item->title .= " <b>{$label}</b>";
		}
	}
	
	return $items;
}, 10, 2);
```