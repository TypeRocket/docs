Title: Conditional Fields
Description: Conditional elements can hide and show their content from view when a target field has a specified value.

---

## Getting Started

*Pro Only: This is a Pro only extension feature.*

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

If you are using groups, you can select multiple groups down using the `<` selector. For each `<` the conditional will look one field group down.

```php
echo $form->toggle('Company Open'); 
echo $form->text('my_group.another_group.Hours')
    ->when('<<company_open');
```

## Select From Root

To look for a field from the root up, use the `/` selector.

```php
echo $form->toggle('Company Open'); 
echo $form->text('my_group.another_group.Hours')
    ->when('/company_open');
```

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