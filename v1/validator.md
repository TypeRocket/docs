Title: Validator
Description: The validator class help you validate fields submitted by a form.

---

## About Validation

There are many times you need to validate fields submitted by a form. Take a look at an example validation:

```php
$request = new \TypeRocket\Http\Request();

$options = [
    'email' => 'required|email',
    'name'  => 'required|min:3'
];

$validator = tr_validator($options, $request->getFields());

$errors = $validator->getErrors();
$passes = $validator->getPasses();
```

Validation is best done for custom resources. Post types fields should not be required but should be expected to be empty just as all other post type fields are in WordPress.

## Get Errors

To get any validation errors use the `getErrors()` method.

```php
$errors = $validator->getErrors();
```

You can also get the errors used for inline fields.

```php
$errors = $validator->getErrorFields();
```

### Redirect On Error

You can [redirect](/docs/v1/redirects) **back** right away, within a controller, when validation fails. When redirecting on error the old fields and error fields needed for inline field errors will get set and sent as well.

```php
// throws a RedirectError
$validator->redirectOnError();
```

You can modify the redirect used by passing a callback to the method.

```php
$validator->redirectOnError(function($redirect) {
	return $redirect->toHome();
});
```

## Get Passing

To get the passing validation rules use the `getPasses()` method.

```php
$passes = $validator->getPasses();
```

## Check Passed

If check if validation passed, use the `passed()` method.

```php
$validator->passed();
$validator->failed();
```

## Multiple Options

To use multiple options, use the `|` (pipe) character to separate them.

## Option Types

There are 9 option types: `required`, `email`, `min:{int}`, `max:{int}`, `size:{int}`, `numeric`, `url`, `callback:{callback}:{[option]}`, and ( `unique:{field}:{[id]}` and `unique:{field}:{table[@column]}{[id]}`).

### Min

To set a character minimum, use the `min` option.

```php
$options = [
    'name'  => 'min:3'
];

$validator = tr_validator($options, $request->getFields());
```

### Min

To set a character minimum, use the `min` option.

```php
$options = [
    'name'  => 'min:3'
];

$validator = tr_validator($options, $request->getFields());
```

### Max

To set a character maximum, use the `max` option.

```php
$options = [
    'name'  => 'max:3'
];

$validator = tr_validator($options, $request->getFields());
```

### Size

To set a character size requirement, use the `size` option.

```php
$options = [
    'state'  => 'size:2'
];

$validator = tr_validator($options, $request->getFields());
```

### URL

To set a URL requirement, use the `url` option.

```php
$options = [
    'website'  => 'url'
];

$validator = tr_validator($options, $request->getFields());
```

### Required

To make a field required, use the `required` option.

```php
$options = [
    'email_address'  => 'required'
];

$validator = tr_validator($options, $request->getFields());
```

### Email

To make a field required to be an email, use the `email` option.

```php
$options = [
    'email_address'  => 'email'
];

$validator = tr_validator($options, $request->getFields());
```

### Unique

To make a field unique use the `unique` option. The unique option has a specific format and requirements. the requirements are:

1. `field` - The database column that should be unique.
2. `id` (optional) - The ID of a row to ignore.

```
unique:{field}:{[id]}
```

#### With Model

For example, in a one to one relationship between a `Seat` and `Person` only one person can have a `Seat` so it must be unique.

```php
$options['persons_id'] = 'unique:persons_id:3';

$validator = tr_validator($options, $request->getFields(), \App\Models\Seat);
```

#### With Table

If you want to check for uniqueness on a table, instead of a model, you can do the following.

```
unique:{field}:{table[@column]}:{[id]}
```

For example, the below will validate and check for uniqueness in the `wp_options` table on the `option_name` column.

```php
$fields['option_name'] = 'mailserver_url';

$validator = tr_validator([
    'option_name' => 'unique:option_name:wp_options'
], $fields);
```

You can also add an ignore check. For example, the below will validate and check for uniqueness in the `wp_options` table on the `option_name` column where the `option_id` does not equal `14`.

```php
$fields['option_name'] = 'mailserver_url';

$validator = tr_validator([
    'option_name' => 'unique:option_name:wp_options@option_id:14'
], $fields);
``` 

### Callback

To make your own validation use the `callback` option. The callback option has a specific format and requirements. the requirements are:

1. `callback` - A callback function for the validator to use on the field value.
2. `option` (optional) - A string you wish to pass to the callback.

```
callback:{callback}:{[option]}
```

For example, in a one to one relationship between a `Seat` and `Person` only one person can have a `Seat` so it must be unique.

```php
// Callback Function
function checkCallback($validator, $value, $field, $option2) {
    if( $value != '' ) {
        return ['success' => $field . ' is good!'];
    } else {
        return ['error' => $field . ' is missing'];
    }  
}

// Validator
$options = [
    'persons_id'  => 'callback:checkCallback:3'
];

$validator = tr_validator($options, $request->getFields(), \App\Models\Seat);

```

## Flash Errors

To flash errors to the admin on the next request use the `flashErrors()` method;

```php
if($validator->getErrors() ) {
     $validator->flashErrors();
}
```

## Validate Grouped Fields

You can also validate deeply embedded fields.

```php
$fields['person'][1]['email'] = 'example@example.com';
$fields['person'][2]['email'] = 'example2.1@example.com';

$validator = tr_validator([
   'person.*.email' => 'email'
], $fields);
```