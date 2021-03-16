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
    protected $run = false;  
  
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