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
    echo $form->repeater('Speakers')->setFields([
        $form->image('Photo'),
        $form->text('Name'),
        $form->text('Slides URL')
    ]);
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

## JavaScript Hook

You can also use the [javascript hooks](/docs/v3/javascript-hooks/) to do something when a repeater field group is added.