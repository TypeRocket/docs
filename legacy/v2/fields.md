Title: Fields
Description: Configure any field exactly how you want it.

---

The `Field` class is used to create custom fields. A `Field` can be anything you want; from a field for a person's “First Name” to a “Matrix”. You can decide what you want your fields to be.

## Instancing a Field

Fields are most commonly instanced, or created, through a `Form` object.

For example, when you use the `Form` method `text()` or `image()`.

```php
// Form instance as $form
$form = tr_form();

// Text Field as $textField  
$textField = $form->text('First Name');

// Image Field as $imageField
$imageField = $form->image('Photo');
```

You can instance fields on their own too. When you instance a `Field` on your own you need to connect it to a `Form` by passing the `Form` object as the last argument.

```php
$form = tr_form();
$textField = new \TypeRocket\Fields\Text('First Name', $form);
$imageField = new \TypeRocket\Fields\Image('Photo', $form);
```

When you use the `Form` object to instance a new `Field`, `$form->text()`, this connection is made for you.

!!!Note: You can pass the `Form` object into a `Field` during construction any place you like.!!!

### Parameters

Each field takes four standard arguments:

1. Name - `string` (required) - This will be the name of the field.
2. Attributes - `array` - This will be the HTML attributes applied to the field.
3. Settings - `array` - This will be custom settings that can very between fields.
4. Label - `boolean` - This is not the label text. Set `false` to disable the label.

Each field also accepts a `Form` object as a last parameter no matter how many other arguments here are.

```php
$attributes = array( 'class' => 'last-name-field' );
$settings = array( 'help' => 'Enter your first name only' );
$useLabel = true;
$form = tr_form();

$fieldFirst = new \TypeRocket\Fields\Text(
    'First Name', 
    $attributes, 
    $settings, 
    $useLabel,
    $form
);

$fieldLast = new \TypeRocket\Fields\Text(
    'Last Name', 
    $form
);
```

## Field Name

To set the field's name after it has been instanced use `setName()`.

```php
$field->setName( 'Last Name' );
```

To get the name use `getName()`.

```php
$name = $field->getName();
```

## Groups

There are times when you want to have fields grouped together when being saved. A great case for this is when you are saving data to `wp_options` using the `OptionsController`.

Use `setGroup()` to group fields together. If you are using grouping with a `Form` you might want to merge the `Form` group with the `Field` group; to nest `Field `group with the `Form` group.

```php
$formGroup = $form->getGroup();

$fieldFirst = new \TypeRocket\Fields\Text(
    'First', 
    $form
);

$fieldLast = new \TypeRocket\Fields\Text(
    'Last', 
    $form
);

$fieldLast->setGroup( $formGroup . '[name]' );
$fieldLast->setGroup( $formGroup . '[name]' );
```

### Bracket Syntax

Groups should be set to a `string` using the TypeRocket bracket syntax. The TypeRocket bracket syntax suggests that you use only lowercase case letters and underscores.

```
[lowercase_underscore_only]
```

### Getting the group

You can also get the current group being used with the method `getGroup()`.

```php
$group = $field->getGroup();
```

## Subgroups

You can also do the inverse of grouping with subgroups. However, there are very few use-cases for doing so.

Typically a subgroup is set with blank brackets. Use the `setSub()` method to set the subgroup. 

```php
$field->setSub('[]');
```

You can also get the subgroup with the `getSub()` `Form` method.

```php
$subgroup = $field->getSub();
```

## Brackets

By default brackets are a combination of the `group + name + subgroup`. The `getBrackets()` method generates brackets for you.

### Example: Getting generated brackets

```php
$field->setName('First')->setGroup('[name]')->setSub('[]');

echo $field->getBrackets();
```

Will output.

```html
[name][first][]
```

## Prefix

The Prefix by default is set to `tr`. It is combined with the brackets to build the inner part of the input fields name attribute.

*Note: If you change the prefix TypeRocket will not know if you have fields. It is not recommended to change the field prefix. `Request` relies on the `tr` prefix for fields.* 

You set the prefix with `setPrefix()` and get the prefix with `getPrefix()`.

### Example 1: Setting a prefix

```php
$field->setName('First');
$field->setPrefix('prefix');
echo $field->getNameAttributeString();
```

Will output.

```html
prefix[first]
```

### Example 2: Default a prefix

```php
$field->setName('First');
$field->setPrefix('prefix');
echo $field->getNameAttributeString();
```

Will output.

```html
tr[first]
```

### Example 3: Setting a Prefix with groups and subgroups

```php
$field->setName('First')->setGroup('[name]')->setSub('[]');
$field->setPrefix('prefix');
echo $field->getNameAttributeString();
```

Will output.

```html
prefix[name][first][]
```

## Populate

When a `Field` is connected to a `Form` the `Field` will populate with the data from the forms `Model`. You can keep a field from being populated by using the method `setPopulate()` to `false`.

```php
$field->setPopulate(false);
```

You can also get the population settings.

```php
$populate = $field->getPopulate();
```

## Builtin Fields

`Models` save fields as either "meta" or "builtin". If a field's name corresponds to a Model's builtin field they will be saved to main resource table columns in the database.

To help you identify what fields are builtin debug mode adds a table icon to field's label.

### Example: Posts builtin fields

For example, the main resource `table` for posts is called `wp_posts`. The builtin fields for the posts resource correspond to the `wp_posts` columns.

A few of these are `post_content`, `post_title`, `post_author`, `ping_status`, `menu_order`, etc.

When a field's name matches a column it is saved to the column.

Take a look at a few ways of setting a field to `post_content`.

```php
$field = new \TypeRocket\Fields\Text('Post Content', $form);
$form->editor('post_content')->setLabel('Content');
$field->setName('Post Content');
```

## Attributes

Attributes are the HTML attributes set to the `Field` when it is converted to a string. Attributes are not always compatible with a `Field`; but common fields like `\TypeRocket\Fields\Text` do.

There are seven methods for working with `Field` attributes.

These methods are: `getAttributes()`, `setAttributes()`, `getAttribute()`, `setAttribute()`, `removeAttribute()`, `appendStringToAttribute()` and `getNameAttributeString()`.

1. `getAttributes()` returns the full array of attributes.
2. `setAttributes()` takes an array of attributes.
3. `getAttribute()` return an attribute by its key.
4. `setAttribute()` sets an attribute by a key.
5. `removeAttribute()` removes an attribute by its key.
6. `appendStringToAttribute()` appends a string to an attribute by key.
7. `getNameAttributeString()` returns the value for HTML `name` attribute.

### Basic attribute methods

Take a look at using all the basic methods.

```php
$args = $field->getAttributes();
$args = array_merge( $args, array( 'id' => 'names' ) );
$field->setAttributes( $args );
$classes = $field->getAttribute( 'class' );
$field->removeAttribute( 'id' );
$field->setAttribute( 'class', $classes );
```

### Appending a string to an attribute

Here we use the `` method to extend the class attribute. Notice that we use an extra space at the beginning.

```php
$field->appendStringToAttribute( 'class', ' date-picker' );
```

### Field name attribute

When using `getNameAttributeString()` the `Field` generates a string that is a combination of `prefix + brackets`. This is most handy when building your own custom fields.

```php
$field->setName('First')->setGroup('[name]')->setSub('[]');
$field->setPrefix('prefix');
echo $field->getNameAttributeString();
```

Will output.

```html
prefix[name][first][]
```

## Settings

There are five methods for dealing with `Field` settings.

These methods are: `getSettings()`, `setSettings()`, `getSetting()`, `setSetting()` and `removeSetting()`.

1. `getSettings()` returns the full array of settings.
2. `setSettings()` takes an array of settings.
3. `getSetting()` return an setting by its key.
4. `setSetting()` sets an setting by a key.
5. `removeSetting()` removes an setting by its key.

Take a look at using all the methods.

```php
$args = $field->getSettings();
$args = array_merge( $args, array( 'render' => 'raw' ) );
$field->setSettings( $args );
$render = $field->getSetting( 'render' );
$field->removeSetting( 'render' );
$field->setSetting( 'render', $render );
```

## Special Settings

Every TypeRocket `Field` has at least two special settings `help` and `render`. Some fields like `\TypeRocket\Fields\Checkbox` and `\TypeRocket\Fields\WordPressEditor` have unique settings.

1. The setting key `render` set to `raw` will render a field without `Form` markup. See rendering below.
2. The setting key `help` set to any string of text will add help text to the field with it is connected to a `Form`.

### Render mode

```php
$field->setSetting('render', 'raw');
```

### Help text

```php
$field->setSetting('help', 'Enter your first name');
```

## Rendering

TypeRocket fields have some standard HTML formatting when they are used with a `Form`. Take a look at the output of a basic text field: `$form->text('Name')`.

```html
<div class="control-section typerocket-fields-text">
    <div class="control-label">
        <span class="label">Name</span>
    <div>
    <div class="control">
        <input type="text" name="tr[name]" value="">
    </div>
</div>
```

### Set render mode

The `Field` object has a setting called render that can be set to `raw`. When the setting render is set to `raw` a `Field` connected to a `Form` will not include any extra HTML. 

The method `setRenderSetting()` is a shortcut method to set the render setting.

```php
echo $form->text('Name')->setRenderSetting('raw');
```

```html
<input type="text" name="tr[name]" value="">
```

This gives you total control of the design when you need it.

### Get render mode

You can also get the render mode by using the method `getRenderSetting()`.

```php
$renderMode = $form->text('Name')->getRenderSetting();
```

## Label

You can set the label of a `Field` with `setLabel()`. By default the `Field` name is used as the label. This method lets you set the label manually.

```php
$field = new \TypeRocket\Fields\Text(
    'Name', 
    $form
);

echo $field->setLabel('First Name');
```

Would output.

```html
<div class="control-section typerocket-fields-text">
    <div class="control-label">
        <span class="label">First Name</span>
    <div>
    <div class="control">
        <input type="text" name="tr[name]" value="">
    </div>
</div>
``` 

### Get the label

You can also get the label with `getLabel()`.

```php
$label = $field->getLabel();
```

### Label option

You can set the label option with the method `setLabelOption()`. The label option lets the `Form` know if it should include the label or not. Setting the option to `false` will disable the label.


```php
$field = new \TypeRocket\Fields\Text(
    'Name', 
    $form
);

echo $field->setLabelOption(false);
```

Would output.

```html
<div class="control-section typerocket-fields-text">
    <div class="control">
        <input type="text" name="tr[name]" value="">
    </div>
</div>
``` 

### Get the label option

You can also get the label option with `getLabelOption()`.

```php
$option = $field->getLabelOption();
```

## Getting the value

To get the value of a `Field` use the `getValue()` method. This method requires that the field is connected to a `Form` so that it can access the form's `Model`. If populate is set to `false` on the `Field` or the `Form` no value will be returned.

```php
$value = $field->getValue();
```