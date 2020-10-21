Title: Repeater Field
Description: The repeater field lets you create groups of repeating fields.

---

## Repeaters

The repeater field lets you create groups of repeating fields.

For example, what if you're building an event listing site and need to list speakers for each event. A repeater field would be perfect for listing the speakers in an event post type since every speaker's information will be different even if they speak at multiple events.

The repeater could group fields for the speakers name, photo and a link to their slides.

Take a look at adding a meta box with the repeater.

```php 
$box = tr_meta_box('Speakers');
$box->addScreen( 'event' );
$box->setCallback(function() {
    $form = tr_form();
    echo $form->repeater('Speakers')->setFields(array(
        array('image', array('Photo')),
        $form->text('Name'),
        array('text', array('Slides URL'))
    ));
});
```

![Repeater field](https://l.rb.typerocket.test/wp-content/uploads/2015/07/docs-repeater-field-typerocket.png)

## Fields

Before we look at how to format a field parameter correctly take a look at the methods we can use. There are three methods for dealing with fields.

### Methods

These methods are: `getFields()`, `setFields()` and `appendField()`.

1. `getFields()` returns the full `array` of field arrays.
2. `setFields()` takes an `array` of field arrays.
3. `appendField()` append a field `array`.

### Format

When you want to add a field to a repeater you need to use a `Field` object or a specifically formatted `array`. The `array` should have two values:

1. First, the `Form` field method you want to call. For example `text` corresponds to `$form->text()`, `\TypeRocket\Form::text`.
2. Second, an array of arguments to pass the method. These are for the field name, attributes, settings and label option.

Here is an example of a repeater appending a field array.

```php
$name = 'Name';
$attributes = array( 'class' => 'speaker-name' );
$settings = array('help' => 'Enter speakers full name');
$useLabel = true;

$formMethod = 'text';
$methodArguments = array( $name, $attributes, $settings, $useLabel );

$nameField = array( $formMethod, $methodArguments );

$form->repeater('Speakers')->appendField($nameField);
```

## JavaScript Hook

You can also use the [javascript hooks](https://l.rb.typerocket.test/docs/javascript-hooks/) to do something when a repeater field group is added.