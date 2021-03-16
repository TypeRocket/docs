Title: Option Fields
Description: Configuring fields with options like select, radio and matrix.

---

Some fields require options such as `\TypeRocket\Fields\Select`, `\TypeRocket\Fields\Radio`, `\TypeRocket\Fields\Matrix`. These fields are call "Option fields".

Any `Field` that has selectable options, like a dropdown list or radio buttons, implements the `\TypeRocket\Fields\OptionField` interface.

When you want to define what a field's options are you need to set them. In the case of the select field setting the options will populate the select dropdown list.

```php
$field = new \TypeRocket\Fields\Select('Email Me', $form);
echo $field->setOptions( array(
    'Yes' => '1',
    'No'  => '0'
) )->setRenderSetting('raw');
```

or,

```php
echo $form->select('Email Me')->setOptions( array(
    'Yes' => '1',
    'No'  => '0'
) )->setRenderSetting('raw');
```

Both will output:

```html
<select name="tr[email_me]">
    <option value="1">Yes</option>
    <option value="0">No</option>
</select>
```

## Options

When setting options the `key` will be the label text and the `value` will be set as the HTML value attribute. The keys and values should *always be set as strings* since they are saved to the database as strings. There are five methods for dealing with options.

These methods are: `getOptions()`, `setOptions()`, `getOption()`, `setOption()` and `removeOption()`.

1. `getOptions()` returns the full array of options.
2. `setOptions()` takes an array of options.
3. `getOption()` return an option by its key.
4. `setOption()` sets an option by a key.
5. `removeOption()` removes an option by its key.

Take a look at using all the methods.

```php
$options = $field->getOptions();
$options = array_merge( $options, array( 'Maybe so' => 'undecided' ) );
$field->setOptions( $options );
$undecided = $field->getOption( 'Maybe so' );
$field->removeOption( 'Maybe so' );
$field->setOption( 'Maybe so', $undecided );
```

## Default value

Option fields also have a special setting called `default`.

When the `default` setting value is the exact same as an option's value the matching option will be selected. However, the default will not override a saved value.

### Example 1: setting a default value

Here is how to set a default properly. The default value is an exact `type` match with an options value. In this case `'0'`.

```php
$field = new \TypeRocket\Fields\Select('Email Me', $form);
$field->setSetting('default', '0');
echo $field->setOptions( array(
    'Yes' => '1',
    'No'  => '0'
) )->setRenderSetting('raw');
```

Will output:

```html
<select name="tr[email_me]">
    <option value="1">Yes</option>
    <option value="0" selected="selected">No</option>
</select>
```

### Example 2: default not exact match

Here is a bad example of setting a default value. Because there is no exact match the default value is not set. `0` is not the same as `'0'`.

```php
$field = new \TypeRocket\Fields\Select('Email Me', $form);
$field->setSetting('default', '0');
echo $field->setOptions( array(
    'Yes' => 1,
    'No'  => 0
) )->setRenderSetting('raw');
```