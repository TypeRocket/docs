Title: Http Fields
Description: Custom HTTP request fields.

---

## Getting Started

HTTP `Fields` provides a quick interface for working with custom fields within an HTTP request.

To get started, create an HTTP field using the galaxy CLI. 

```shell
php galaxy make:fields MyFields
```

This will generate the following unser `app/Http/Fields/MyFields`:

```php
<?php  
namespace App\Http\Fields;  
  
use TypeRocket\Http\Fields;  
  
class MyFields extends Fields  
{  
    /** @var bool Validate & redirect on errors immediately */  
    protected $run = null;  
  
    /**  
     * Extend the fillable fields of a model
     * these fields are used with.
     * 
     * @return array 
     */
    protected function fillable() {  
        return [];  
    }  
  
    /**
	 * Validator rules these fields must
	 * pass.
	 *
     * @return array 
     */  
     protected function rules() {  
        return [];  
     }  
}
```

## Within a Controller

You can then use the fields within a controller. For example, you could use the field to get the field used on a POST request:

```php
tr_route()->post()->on('submit', function(\App\Http\Fields\MyFields $fields) {  
    (new \App\Models\Post)->save($fields);  
    return tr_redirect()->back();  
});
```

## Execute Validation and Redirect

To run field validation and redirect with errors use the fields class method`validate()` and the validator classes `redirectWithErrorsIfFailed()`. Running validation and redirecting before saving data will allow the validation to exit before saving if there are validation errors.

```php
tr_route()->post()->on('submit', function(\App\Http\Fields\MyFields $fields) {
    $fields->validate()->redirectWithErrorsIfFailed()
    (new \App\Models\Post)->save($fields);  
    return tr_redirect()->back();
});
```

### Auto Execute And Redirect

When a fields class, like `MyFields`, is imported into a controller you can force the field class to automatically validate based on its rules. To auto execute and redirect set the fields class' `$run` property to `true`.

If you want to modify the redirect add a `redirect()` method to the field class.

```php
public function redirect(\TypeRocket\Http\Redirect $redirect)
{
    $redirect->toHome();

    return $redirect;
}
```