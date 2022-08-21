Title: Conditional Fields
Description: Conditional elements can hide and show their content from view when a target field has a specified value.

---

## Getting Started

!!

If you are coming from another custom field plugin, you probably use conditional fields. TypeRocket Pro has conditional fields and many other conditional elements:

- **Fields**
- Rows
- Sections
- Fieldsets

**Conditional elements** can hide and show their content from view **when** the value of a target field, or fields, matches the requirements of the conditional elements.

The `when()` method is for declaring an element as conditional. Here is a simple example:

```php
echo $form->toggle('Company Open');  
echo $form->text('Hours')->when('company_open');
```

When the above toggle element is on and `true`, the text field `hours` will be displayed on the page. When the toggle is off or `false`,  the field `hours` will be hidden.

When a field is hidden its **value is saved to the database**. Conditional elements **only hide from view**, but their HTML input values are still submitted with the form.

<iframe width="560" height="315" src="https://www.youtube.com/embed/eOjK4ntDfE4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Rows

```php
echo $form->toggle('Company Open');

echo $form->row(  
    $form->text('Day'),  
    $form->time('Start Time'),  
    $form->time('End Time')  
)->when('company_open');
```

### Fieldsets

```php
echo $form->toggle('Company Open');

echo $form->fieldset('Title', 'Description',   
    $form->text('Day'),  
    $form->time('Start Time'),  
    $form->time('End Time')  
)->when('company_open');
```

### Sections

```php
echo $form->toggle('Company Open');

echo $form->section(  
    $form->text('Hours')
)->when('company_open');
```

## Logic & Operators

- String value `contains` - case insensitive.
- Number is `>`.
- Number is `<`.
- Value is `=`.
- JavaScript `function` name. A function accepting the value and rule as arguments. Returns `true` or `false`.

### Contains

```php
echo $form->text('Company Name');

echo $form->section(  
    $form->image('TypeRocket Logo')
)->when('company_name', 'contains', 'TypeRocket');
```

### Standard Operators

```php
echo $form->select('Number')->setOptions([1 => 1, 2 => 2]);

echo $form->toggle('Feature It')->when('number', '=', 1);
echo $form->toggle('Feature This')->when('number', '<', 2);
echo $form->toggle('Feature That')->when('number', '>', 1);
```

### JavaScript Function

```js
// JS
function my_function(field_value, rule_value) {
    return field_value === rule_value;
}
```

```php
// PHP
echo $form->select('Number')->setOptions([1 => 1, 2 => 2]);
echo $form->toggle('Feature It')->when('number', 'my_function', 1);
```

## Multiple Conditions

You can use any number of `orWhen()` and `when()` methods on a single conditional element. 

```php
echo $form->toggle('Company Open');
echo $form->select('Number')->setOptions([1 => 1, 2 => 2]);
 
echo $form->text('Hours')
    ->when('company_open')
    ->orWhen('number', '>', 1);
```

## Select Child Fields

If you are using groups, you can select multiple groups down using the `<` subfield selector. For each `<` the conditional will look one field group down.

```php
echo $form->toggle('Company Open'); 
echo $form->text('my_group.another_group.Hours')
    ->when('<<company_open');
```

You can use subfield selector `<` to access main level fields from within repeater, matrix, and builder fields. For example:

```php
echo $form->toggle('Use Descriptions'); 

echo $form->repeater('Advantages')->setFields([
    $form->text('Title'),
    $form->text('Description')->when('<<use_descriptions'),
])->setLimit(4);
```

When using a repeater you must go back two levels by prefixing with `<<` (go back two subfields). The first level is the element index `1738502764832` so dive one level with the first `<` and the next is the field name `advantages` so dive one more level with `<`. Finally, access the field with `use_descriptions`. So, the "when" lookup target is `<<use_descriptions`.

## Select From Root

To look for a field from the root up, use the `/` selector.

```php
echo $form->toggle('Company Open'); 
echo $form->text('my_group.another_group.Hours')
    ->when('/company_open');
```

## Modes

To set the conditional field mode use the method `conditionMode`. Conditional fields have two modes: "display" and "include".

1. `display` - This is the default mode. When using display mode, the field will be hidden from view using CSS. This mode does not stop the field's value from being sent when the form is submitted.
2. `include` - When using include mode, the field's name attribute will be disabled, and the display is also hidden using CSS. In addition, this mode stops the field's value from being sent when the form is submitted.

For example, you can use the `conditionMode('include')` to have a user toggle between two field types that share the same name depending on context.

```php
echo $form->toggle('Background');
echo $form->image('Image')->when('background', false)->conditionMode('include');
echo $form->background('Image')->when('background')->conditionMode('include');
```

*Note: Using the `include` mode can result in data loss because this mode is designed to exclude data when the condition is not met. Use the `display` mode to retain all data.*

## Valid Fields

Conditional elements can target not all fields. Fields like gallery, items, matrix, builder, and others. These fields are typically fields that do not return a single value.

Valid target fields include:

- Radio
- Toggle
- Checkbox
- Input
- Password
- Textarea
- Swatches
- Image
- Text
- Search (single)
- Select (single)