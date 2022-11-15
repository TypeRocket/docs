Title: Validator
Description: The validator class help you validate fields submitted by a form.

---

## About Validation

There are many times you need to validate fields submitted by a form. Take a look at an example validation:

```php
$options = [
    'email' => 'required|email',
    'name'  => 'required|min:3'
];

$validator = tr_validator($options, tr_request()->getFields())->validate(true);

$failed = $validator->failed();
$passed = $validator->passed();
```

Validation is best done for custom resources. Post types fields should not be required but should be expected to be empty just as all other post type fields are in WordPress. However, you can [implement field validation for post types](/add-custom-field-validation-to-wordpress-post-types/).

## Using Errors

To get all validation errors use the `getErrors()` method. This will return an array of stings with the error messages.

```php
$errors = $validator->getErrors();
```

### Inline Form Field Errors

You can also get the errors used for inline form fields. This will return an array of stings with the error messages. These errors are shortened and do not include the field name.

```php
$errorsForFields = $validator->getErrorFields();
```

Once getting the inline field errors, to display them, send them back to the form by redirecting the response. If you want the errors to include the field labels use `$validator->getErrors()` instead of `$validator->getErrorFields()`.

```php
tr_redirect()->withErrors( ['fields' => $errorsForFields] )->back()->now();
```

Then in your form use `useErrors()`.

```php
tr_form()->useErrors();
```

### Redirect On Error

You can [redirect](/docs/v6/redirects/) **back** right away, within a controller, when validation fails. When redirecting and validations fails a flash message, the old fields, and inline field errors will get set and sent as well.

```php
// throws a RedirectError
$validator->redirectWithErrorsIfFailed();
```

You can modify the redirect used by passing a callback to the method.

```php
$cb = function($redirect) {
	return $redirect->toHome();
};

$validator->redirectWithErrorsIfFailed($cb);
```

Also, you can disable the error flash messages by passing `false` as the second argument.

```php
$validator->redirectWithErrorsIfFailed($cb, false);
```

### Flash Errors

To flash errors to the admin on the next request use the `flashErrors()` method. This will flash the errors on the next request. So, you will need to redirect if validation has errors.

```php
if($validator->failed() ) {
     $validator->flashErrors();
     
     tr_redirect()->back()->now();
}
```

If you need the errors to flash on the front-end use the `\TypeRocket\Elements\Notice` class.

```php
echo \TypeRocket\Elements\Notice::flash();
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

## Validation Rules

There are 10 validation Rules: `required`, `key`, `email`, `min:{int}`, `max:{int}`, `size:{int}`, `numeric`, `url`, `callback:{callback}:{[option]}`, and ( `unique:{field}:{[id]}` and `unique:{field}:{table[@column]}{[id]}`). You can make custom validation rules if you need more.

### Min

To set a character minimum, use the `min` option.

```php
$rules = [
    'name'  => 'min:3'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### Max

To set a character maximum, use the `max` option.

```php
$rules = [
    'name'  => 'max:3'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### Size

To set a character size requirement, use the `size` option.

```php
$rules = [
    'state'  => 'size:2'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### Key 

May only contain lowercase alphanumeric characters and underscores.

```php
$rules = [
    'name'  => 'key'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### URL

To set a URL requirement, use the `url` option.

```php
$rules = [
    'website'  => 'url'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### Required

To make a field required, use the `required` option.

```php
$rules = [
    'email_address'  => 'required'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

To require a field but allow for `NULL`, use the `weak` option.

```php
$rules = [
    'email_address'  => 'required:weak'
];
```

To require a field but first `trim()` the value(s), use the `strong` option.

```php
$rules = [
    'email_address'  => 'required:strong'
];
```

To require a field and allow the value of `0`, use the `allow_zero` option.

```php
$rules = [
    'email_address'  => 'required:allow_zero'
];
```

To combine options separate them with a `/`.

```php
$rules = [
    'email_address'  => 'required:strong/allow_zero'
];
```

### Datetime Local

To make a field required to be an in the [HTML5 input datetime-local field](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/datetime-local) format `<input type="datetime-local">`, use the `datetime-local` option.

```php
$rules = [
    'datetime'  => 'datetime-local'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

### Email

To make a field required to be an email, use the `email` option.

```php
$rules = [
    'email_address'  => 'email'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
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
$rules['persons_id'] = 'unique:persons_id:3';

$validator = tr_validator($rules, tr_request()->getFields(), \App\Models\Seat)->validate(true);
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
function checkCallback($args) 
{
    /**
     * @var $option3
     * @var $option
     * @var $option2
     * @var $name
     * @var $field_name
     * @var $value
     * @var $type
     * @var \TypeRocket\Utility\Validator $validator
     */
    extract($args);

    $error = null;

    if( empty($value) ) {
        return $field_name . ' is bad'; // has error
    }

    return true; // passed
}

// Validator
$rules = [
    'persons_id'  => 'callback:checkCallback:3'
];

$validator = tr_validator($rules, tr_request()->getFields())->validate(true);
```

## Validate Grouped Fields

You can also validate deeply embedded fields. This will require the group to be present for the value to validate.

```php
$fields['person'][1]['email'] = 'example@example.com';
$fields['person'][2]['email'] = 'example2.1@example.com';

$rules = [
   'person.*.email' => 'email'
];

$validator = tr_validator($rules, $fields)->validate(true);
```

By using `?` instead of `*` you can weakly validate a group. The `?` wildcard is helpful if the group is not required but you still want to validate the fields within the group when the group is there.

```php
// Will still pass validation.
$fields['person'] = [];

$rules = [
   'person.?.email' => 'required|email'
];

$validator = tr_validator($rules, $fields)->validate(true);
```

## Custom Validation Rules

You will need to extend the `\TypeRocket\Utility\Validators\ValidatorRule` class to create custom rules. You can make a custom validation rule using the Galaxy CLI using the command `php galaxy make:rule <key> <class>`. For example, you might make a variation of the email validation rule with `php galaxy make:rule domain_email DomainEmailValidator`:

```php
<?php
namespace App\Validators;

use TypeRocket\Utility\Str;
use TypeRocket\Utility\Validator;

class DomainEmailValidator extends ValidatorRule
{
    public CONST KEY = 'domain_email';

    public function validate(): bool
    {
        /**
         * @var $option
         * @var $option2
         * @var $option3
         * @var $full_name
         * @var $field_name
         * @var $value
         * @var $type
         * @var Validator $validator
         */
        extract($this->args);

        if( ! filter_var($value, FILTER_VALIDATE_EMAIL) && ! Str::contains('example.com', $value) ) {
            $this->error = "must be an example.com email address.";
        }

        return !$this->error;
    }
}
```

Now you can validate using the class and combine it with other rules:

```php
$validator = tr_validator([
   'person.*.email' => [\App\Validators\DomainEmailValidator::new(), 'required']
], $fields)->validate(true);
```

You can add your own validation rules with the `typerocket_validator_map` filter so you can access them by key. You can also use this feature to replace an existing rule with your own.

```php
add_filter('typerocket_validator_map', function($map) {
    $map[\App\Validators\DomainEmailValidator::KEY] = \App\Validators\DomainEmailValidator::class;
    return $map;
});
```

Now you can access it by key:

```php
$validator = tr_validator([
   'person.*.email' => 'domain_email|required'
], $fields)->validate(true);
```

## Custom Error Messages

You can set custom error messages with `setErrorMessages()`. When using this method `{error}` will be replaced by the validation rules error message. `{error}` is not required.  When setting error messages the key will be a regular expression matched to the field name passed with a `:` and the rule key as a suffix.

```php
$fields['name'] = 'kevin@example.com';
$fields['repeater'][2849573629]['name'] = 'TypeRocket';

$rules = [
    'name' => 'required|email',
    'repeater.?.name' => 'required',
];

$messages = [
    'name:required' => 'Name is a must have!',
    'name:email' => 'Email {error}',
    'repeater.\d+.name:required' => 'Repeater name {error}',
];

$validator = tr_validator($rules, $fields)->setErrorMessages($messages, true)->validate(true);
```

## Add Static Error Message

You can manually apply an error message to the errors list with `appendToFlashErrorMessage()` and `prependToFlashErrorMessage()`.

```php
$validator = tr_validator($rules, $fields)->validate(true);

$validator->appendToFlashErrorMessage('An error message appended.');
$validator->prependToFlashErrorMessage('An error message prepended.');
```

## Multiple Options

To use multiple options, use the `|` (pipe) character to separate them.

```php
$rules = [
    'email' => 'required|email',
];
```

## Weak Validation

To skip validation when a field is not provided or is blank prepend `?` to the validation rule set.

```php
$rules = [
    'email' => '?required|email',
];
```