Title: Tabs
Description: Create tabbed layouts to enhance the design and usability of your forms.

---

## Getting Started
 
If you use the theme options plugin, you will notice that there is a tabbed layout. You can create tabbed layouts with TypeRocket using the `tr_tabs()` function.

```php
$tabs = tr_tabs();
```

Once you have a tabs instance, you can start adding tabs using the `tab()` method. The tab method takes three arguments.

1. Tab Label - The tabs label as a `string`.
2. Icon Name - The name of an icon or `null` for no icon.
3. Content - This can be a `string`, `callback`, `Fieldset`, or an `array` of `Field` objects. 

```php
$tabContentOne = "<p>Main content 1.</p>";
$tabContentTwo = "<p>Main content 2.</p>";

$tabs->tab('Tab 1', 'users', $tabContentOne);
$tabs->tab('Tab 2', 'books', $tabContentTwo);

echo $tabs;
```

## Single Tab

You can work with individual tabs after adding them.

```php
$tabs = tr_tabs();
$tab = $tabs->tab('Books', 'books');
```

Now you can use the `$tab` variable to chain method.

### Set Description

```php
$tabContent = "List of books";
$tab->setDescription($tabContent);
```

### Set Icon

```php
$tab->setIcon('users');
```

### Set Content

```php
$tabContent = "<p>Main content 2.</p>";
$tab->apply($tabContent);
```

### Set Active

```php
$tab->setActive(true);
```

### Set URL

Go to a URL instead of opening the tab on the same page.

```php
$tab->setUrl('https://example.com/wp-admin/my-url');
```

## Tab For Repeaters

```php
$tabs = tr_tabs();
$tabs->tab('Date')->setFields($this->form->date('Date'));
$tabs->tab('Info')->setFields($this->form->text('Info'));
             
echo $form->repeater('Example')->setFields($tabs);
```

## Layout Options

```php
$tabs->layoutTopEnclosed();
$tabs->layoutLeftEnclosed();
$tabs->layoutTop();
$tabs->layoutLeft();
```

## Sidebar

You can add a sidebar to main content tabs using the `setSidebar()` method.

```php
$sidebar = "<p>Sidebar content.</p>";
$tabs->setSidebar($sidebar);
```

## Footer

You can add a footer to main content tabs using the `setFooter()` method.

```php
$footer = "<p>Footer content.</p>";
$tabs->setFooter($footer);
```

## Title

You can add a title to main content tabs using the `setTitle()` method.

```php
$title = "Title content.";
$tabs->setTitle($title);
```

## Only Icons

Only show the icons of tabs.

```php
$tabs->onlyIcons();
```