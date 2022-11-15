Title: Admin Pages
Description: Making and adding admin pages to the WordPress admin.

---

## Admin Pages

Admin pages are located in the WordPress admin in the main sidebar navigation. Admin pages can be used for any purpose. Commonly they are for building custom functionality that requires it's own administrative section.

## Adding an Admin Page

To register an "Admin Page" in WordPress, you only need one line of code.

```php
tr_page('Api', 'view', 'APIs Page');
```

Or, the same using OOP.

```php
\TypeRocket\Register\Page::add('Api', 'view', 'APIs Page');
```

This one line of code adds the "Admin Page" to the WordPress admin, sets all the correct labels in the navigation and applicable places, and implements the required WordPress hooks. This would typically take many lines of code.

! **Note**: By default, anyone who is logged-in with the capability `administrator` can view a page. You can change this later.

### Function Arguments

The `tr_page()` function takes 4 arguments:

1. `$resource` - A string set to the resource or section the page belongs to.
2. `$action` - A string set to the action the page is responsible for.
3. `$title` - A string set to the title of the page and menu.
4. `$settings` - (optional) Array with keys `menu`, `capability`, `position`, `view`, `slug`.
5. `$handler` - (optional) A controller class name as a string or callable to handle the request.

Take a look at creating an admin page.

```php
$settings = ['capability' => 'administrator'];
$seat_index = tr_page('Seat', 'index', 'Plane Seats', $settings);
```

Or, a page with a custom function handler.

```php
$settings = ['capability' => 'read'];
$fn = function() {  
  return 'hi';  
};

tr_page('Api', 'view', 'APIs Page', $settings, $fn);
```

Or, with a controller.

```php
$settings = ['capability' => 'read'];
$controller = '\MyPlugin\Controllers\ApiController';
tr_page('Api', 'view', 'APIs Page', $settings, $controller);
// Calls the `view` method on the controller
```

## Set Icon

To set an icon for the admin page use the `setIcon()` method. Use a [WordPress dashicon](https://developer.wordpress.org/resource/dashicons/#heart).

```php
$seat_index->setIcon('dashicons-heart');
```

## Set Parent Pages

To set a parent page use the `setParent()` method.

```php
$settings = ['capability' => 'editor'];
$add_seat = tr_page('Seat', 'add', 'Add Seat', $settings);
$add_seat->setParent($seat_index);
```

! **Note**: By default, child pages will inherit the parent pages permission or capability settings unless the child has a capability set.

## Add Child Page

You can set child page with the `addPage()` and `apply()` methods.

### Using Add Page

```php
$settings = ['capability' => 'administrator'];
$add_seat = tr_page('Seat', 'add', 'Add Seat', $settings);
$seat_index->addPage($add_seat);
```

### Using Apply

```php
$settings = ['capability' => 'administrator'];
$add_seat = tr_page('Seat', 'add', 'Add Seat', $settings);
$seat_index->apply($add_seat);
```

## "Add New" Page Button

If you have a page with the action "add" you can use the `addNewButton()` to have an add button appear at the top of related admin pages.

```php
$seat_index->addNewButton();
```

This will add an "Add New" button to the top of the seat index page and automatically find the page with the "add" action and link the button to it.

## Add To Admin Bar

To add a page to the admin bar use the `adminBar()` method. By default, this will place items under the "New" dropdown. 

```php
$add_seat->adminBar('add_seat_page');
```

The `adminBar()` method takes 3 arguments:

1. `$id` - The ID to use in the admin bar for the menu.
2. `$title` - The title of the admin bar link.
3. `$parent_id` - The ID of the parent item in the admin bar.

## Set Controller

To have an admin page use a controller pass to the `setHandler()` method the class name as a string. For example, if the add seat page calls the `setHandler()` method it will use the `SeatController`.

```php
$add_seat->setHandler(\App\Controllers\SeatController::class);
``` 

By default, the mapped action is used based on the response type. We will get into mapping actions soon. For now, know the action is the method the controller will call.

### Make Controller

If you're are using a controller for your pages you need to create a controller. To make a seat controller and model for the "Seat" resource use the TypeRocket Galaxy CLI.

```bash
php galaxy make:model -c base Seat
```

! **Note**: Pages work best with controllers so reach for them.

### Map Actions

To take full advantage of the controller, you can map HTTP request methods to specific class functions on the controller. You will need to map actions that are not `add`, `index`, `delete`, `edit`, `create`, `update`, or `show` since only these are mapped automatically.

```php
$settings = ['capability' => 'administrator'];
$seat_ticket = tr_page('Seat', 'ticket', 'Tickets', $settings);
$seat_ticket->setHandler(\App\Controllers\SeatController::class);
$seat_ticket->mapAction('GET', 'ticket');
$seat_ticket->mapAction('POST', 'create_ticket');
```

When using admin pages your options for controllers are somewhat limited, due to the pattern WordPress uses for making routes within the `wp-admin`. The `mapAction` method allows you to take more control over how the admin routes HTTP requests to your controller class.

## Remove Menu

There are times when you want to add a page as a child page but not have it appear in the admin menu. To accomplish this use the `removeMenu()` method.

```php
// create page
$settings = ['capability' => 'administrator'];
$view_seat = tr_page('Seat', 'view', 'View Seat', $settings);

// remove menu
$view_seat->removeMenu();

// add as child page
$seat->apply($view_seat);
```

## Arguments

There are five methods for dealing with arguments. Arguments are used when the page is being registered. The available arguments are: `menu`, `capability`, `inherit_capability`, `position`, `view_file`, and `slug`.

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without affecting the already set values.

```php
$args = $book->getArguments();
$args = array_merge( $args, [ 'position' => 20 ] );
$book->setArguments( $args );
$public = $book->getArgument( 'position' );
$book->removeArgument( 'position' );
$book->setArgument( 'position', 20 );
```

## Position

A parent page is placed in the menu based on its `position` argument. By default, the position is '25`. You see a list of [menu positions in the WordPress developer reference guide](https://developer.wordpress.org/reference/functions/add_menu_page/#menu-structure).

- `2` for Dashboard
- `4` for Separator
- `5` for Posts
- `10` for Media
- `15` for Links
- `20` for Pages
- `25` for Comments
- `59` for Separator
- `60` for Appearance
- `65` for Plugins
- `70` for Users
- `75` for Tools
- `80` for Settings
- `99` for Separator

```php
$page->setPosition(25);
```

## Capability

Set the `capability` setting to a value from the [WordPress capability levels](https://codex.wordpress.org/Roles_and_Capabilities). This will restrict access to the page based on the permissions level.

You can also set the values to a role level. This is many times the simplest and most effective method. The role levels are:

- `administrator`
- `editor`
- `author`
- `contributor`
- `subscriber`

```php
$page->setCapability('administrator');
```

### Inherit Capability

By default, the `inherit_capability` setting is set to true. If a page is a child page, and it has this setting, it will inherit the parent pages `capability` setting if the child page has no capability set. 

## Titles

The `menu` argument setting controls the menu title. This is not the page title.

You can also use these methods to control the titles used by an admin page.

```php
$page->setTitle('Page Title');
$page->setMenuTitle('Main Menu');
$page->setSubMenuTitle('Sub Menu'); // If is sub page
```

## Views

The `view` setting controls what file will be used as the page's template. You can use the `View` using `tr_view()`. Also, you can pass callable, file path, or text block.

```php
$view = tr_view('my.view', ['name' => 'Kevin']);
$page->setView($view);
```

```php
$fn = function() {
	return 'Hi!';
};
$page->setView($fn);
```

```php
$view = __DIR__ . '/my-file.php';
$page->setView($view);
```

```php
$page->setView('Hi there');
```

## Set Slug

The `slug` setting controller the admin pages URL "page" parameter. For example, a slug with the value of "my_page" will appear in the URL as `http://example.com/wp-admin/admin.php?page=my_page`.

```php
$page->setSlug('my_page');
```