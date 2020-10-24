Title: Tabs
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

## Tab Icons (v3.0.16)

Since version 3.0.16 you can now add an icon to tabs by passing a third option.

- `$icon` takes a `string` of the name of an icon from the [Icons system](/docs/v3/icons/). 

```php
$tabs->addTab('Tab 1', $tabContentOne, 'cog');
```

## Tab Callback (v3.0.24)

Since version 3.0.24 you can now use a call back for the content of a tab instead of a string. To do this pass a callback as the second option instead of a string.

```php
$about = function() use ($form) {
    echo $form->text('Company Name');
    echo $form->text('Company Email');
    echo $form->text('Company Phone');
    echo $form->search('Terms Page')->setPostType('page');
    echo $form->checkbox('Company Open')->setText('Company open for business')->setLabel(false);
};

tr_tabs()->addTab( 'About', $about )->render();
```

## Tab For Repeaters (v3.0.24)

Since version 3.0.24 you can now add tabs to repeaters. To do tabs to repeater fields use the `setTabFields($name, $fields)` method.

- `$name` takes a `string` of the name of the tabs to place the fields.
- `$fields` takes an `array` of fields. 

```php
$links = [
    tr_tabs()->setForm($form)->bindCallbacks()
             ->addTab('Date')
             ->setTabFields('Date', [$this->form->date('Date')])
             ->addTab('Info')
             ->setTabFields('Info', [$this->form->text('Info')])
];

echo $form->repeater('Example')->setFields($links);
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