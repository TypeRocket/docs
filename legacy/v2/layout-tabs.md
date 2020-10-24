Title: Layout Tabs
Description: Create tabbed layouts to enhance the design and usability of your forms.

---

If you use the theme options plugin you will notice that there is a tabbed layout. You can create tabbed layouts with TypeRocket using the `tr_tabs()` function.

This function creates an instance of `TypeRocket\Tabs` so you can easily create tabbed layouts.

```php
$tabs = tr_tabs();

$tabContentOne = "<p>Main content 1.</p>";
$tabContentTwo = "<p>Main content 2.</p>";

$tabs->addTab('Tab 1', $tabContentOne);
$tabs->addTab('Tab 2', $tabContentTwo);

$tabs->render();

```

## Meta box tabs

When tabs are added to a meta box they will have this style.

![Tabs meta box](https://typerocket.com/wp-content/uploads/2015/08/docs-tabs-meta-box.png)

## Main content tabs

When tabs are added in the main content area of the admin they will have this style.

![Tabs main content](https://typerocket.com/wp-content/uploads/2015/08/docs-tabs-main-content.png)

## Main content box tabs

When you set the render mode to `box` tabs will have this style.

```php
$tabs->render( 'box' );

```

![Tabs main content box](https://typerocket.com/wp-content/uploads/2015/08/docs-tabs-main-content-box.png)

## Sidebars

You can add a sidebar to main content tabs using the `setSidebar()` method.

```php
$tabs = tr_tabs();

$sidebar = "<p>Sidebar content.</p>";

$tabs->setSidebar( $sidebar );

$tabContentOne = "<p>Main content 1.</p>";
$tabContentTwo = "<p>Main content 2.</p>";

$tabs->addTab('Tab 1', $tabContentOne);
$tabs->addTab('Tab 2', $tabContentTwo);

$tabs->render('box');

```