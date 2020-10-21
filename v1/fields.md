Title: Fields
Description: Configure any field exactly how you want it.

---

## About Fields

The `Field` class is used to create custom fields. A `Field` can be anything you want, from a field for a person's "First Name" to a matrix of information. You can decide what you want your fields to be.

## Instancing a Field

Fields are most commonly instanced or created through a `Form` object. For example, when you use the `Form` method `text()` or `image()`.

```php
// Form instance as $form
$form = tr_form();

// Text Field as $textField  
$textField = $form->text('First Name');

// Image Field as $imageField
$imageField = $form->image('Photo');
```

You can instance fields on their own too. When you instance a `Field` on your own, you need to connect it to a `Form` by passing the `Form` object as the last argument.

```php
$form = tr_form();
$textField = new \TypeRocket\Fields\Text('First Name', $form);
$imageField = new \TypeRocket\Fields\Image('Photo', $form);
```

When you use the `Form` object to instance a new `Field`, `$form->text()`, this connection is made for you.

*Note: You can pass the `Form` object into a `Field` during construction as any argument position you like.*

### Parameters

Each field takes four standard arguments:

1. **Name** - `string` (required) - This will be the name of the field.
2. **Attributes** - `array` - This will be the HTML attributes applied to the field.
3. **Settings** - `array` - This will be custom settings that can vary between fields.
4. **Label** - `boolean` - This is not the label text. Set `false` to disable the label.

Each field also accepts a `Form` object as the last parameter no matter how many other arguments here are.

```php
$attributes = [ 'class' => 'last-name-field' ];
$settings = [ 'help' => 'Enter your first name only' ];
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

To set the field's name after it has been instanced, use `setName()`.

```php
$field->setName( 'Last Name' );
```

To get the name use `getName()`.

```php
$name = $field->getName();
```

## Groups

*Note: Field and From groups are independant.*

There are times when you want to have fields grouped together when being saved. A great case for this is when you are saving data to `wp_options` using the `OptionController`.

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
$fieldLast->setGroup( $formGroup . '.name' );
$fieldLast->setGroup( $formGroup . '.name' );
```

### Getting the group

You can also get the current group being used with the method `getGroup()`.

```php
$group = $field->getGroup();
```

### Extending

```php
$form = tr_form();
$form->text('First Name')->extend('my_group');
// name will be `my_group.first_name`
```

Or, shorthand.

```php
$form->text('my_group.First Name');
// name will be `my_group.first_name`
```

Apply to all fields in the form.

```php
$form = tr_form()->setGroup('my_group');
$form->text('First Name');
// name will be `my_group.first_name`
$form->text('Last Name');
// name will be `my_group.last_name`
$form->text('Last Name')->setGroup('calling');
// name will be `my_group.calling.last_name`
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

## Built-in Fields

`Models` save fields as either "meta" or "builtin". If a field's name corresponds to a Model's builtin field they will be saved to main resource table columns in the database.

To help you identify what fields are builtin, debug mode adds a table icon to the field's label.

### Example: Posts builtin fields

For example, the primary resource `table` for posts is called `wp_posts`. The builtin fields for the posts resource correspond to the `wp_posts` columns.

A few of these are `post_content`, `post_title`, `post_author`, `ping_status`, `menu_order`, etc.

When a field's name matches a column, it is saved to the column.

Take a look at a few ways of setting a field to `post_content`.

```php
$field = new \TypeRocket\Fields\Text('Post Content', $form);
$field->setName('Post Content');
```

Or,

```php
$form->editor('post_content')->setLabel('Content');
```

## Attributes

Attributes are the HTML attributes set to the `Field` when it is converted to a string. Attributes are not always compatible with a `Field`, but common fields like `\TypeRocket\Fields\Text` do.

There are seven methods for working with `Field` attributes.

These methods are: `getAttributes()`, `setAttributes()`, `getAttribute()`, `setAttribute()`, `removeAttribute()`, `appendStringToAttribute()` and `getNameAttributeString()`.

1. `getAttributes()` returns the full array of attributes.
2. `attrReset()` takes an array of attributes and replaces the old.
3. `attrExtend()` takes an array of attributes and merges the old.
4. `getAttribute()` return an attribute by its key.
5. `setAttribute()` sets an attribute by a key.
6. `removeAttribute()` removes an attribute by its key.
7. `attrClass()` appends a string to the class attribute.
8. `attr()` sets and gets values. 
9. `getNameAttributeString()` returns the value for HTML `name` attribute.

### Basic attribute methods

Take a look at using some of the basic methods.

```php
$args = $field->getAttributes();
$field->attrExtend( [ 'class' => 'names' ] );
$classes = $field->attr( 'class' );
$field->removeAttribute( 'class' );
$field->attr( 'class', $classes );
```

*Note: TypeRocket needs to control the ID attribute so it does not give access to the ID attribute.*

### Appending a string to the class attribute

Here we use the `attrClass()` method to extend the class attribute.

```php
$field->attrClass('date-picker');
```

### Field name attribute

When using `getNameAttributeString()` the `Field` generates a string that from the dot notation. This is most handy when building your own custom fields.

```php
$field->setName('First')->setGroup('group_name');
echo $field->getNameAttributeString();
// outputs... tr[group_name][first]
```

## Settings

There are five methods for dealing with `Field` settings.

These methods are: `getSettings()`, `setSettings()`, `getSetting()`, `setSetting()` and `removeSetting()`.

1. `getSettings()` returns the full array of settings.
2. `setSettings()` takes an array of settings.
3. `getSetting()` return an setting by its key.
4. `setSetting()` sets an setting by a key.
5. `removeSetting()` removes an setting by its key.
6. `maybeSetSetting()` set value only if index does not exist.

Take a look at using all the methods.

```php
$args = $field->getSettings();
$args = array_merge( $args, [ 'render' => 'raw' ] );
$field->setSettings( $args );
$render = $field->getSetting( 'render' );
$field->removeSetting( 'render' );
$field->setSetting( 'render', $render );
```

## Help Text

The setting key `help` set to any string of text will add help text to the field with it is connected to a `Form`.

```php
$field->setHelp('Enter your first name');
```

## Label

You can set the label of a `Field` with `setLabel()`. By default, the `Field` name is used as the label. This method lets you set the label manually.

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

## Required Fields

It is best not to make fields required using HTML5 when a form is long or complex. In almost all cases, the backend should be used for validation.

To visually mark fields as required, TypeRocket provides a `markLabelRequired()` method that will mark a field's label with an asterisk: `*`. 

```php
 $field->markLabelRequired();
```

To make a field required with HTML5 use the `setAttribute()` method.