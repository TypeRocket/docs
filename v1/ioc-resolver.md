Title: Services & DI Container
Description: Inversion of control resolver automatically tries to instance a class with its dependencies.

---

## Services 

Services are helpful when you need to register a class with the IoC Container each time your site loads. For example, if you need a service for connecting to an external API like Stripe or Mailgun.

To create a service, use the `make:service` command.

```
php galaxy make:service MyService
```

Will create:

```php
<?php  
namespace App\Services;  
  
use TypeRocket\Services\Service;  
  
class MyService extends Service  
{  
    protected $singleton = true;
    
    public function __construct(){
		// Runs when service is loaded into the container
		// this happens on every page load.
	}

	public function register() : Service  
	{  
	    // Runs when first instance is created
	    // this happens only when requested
	    return $this; 
	}
}
```

Then register the service to your `app.services` config file.

```php
/*  
|--------------------------------------------------------------------------  
| Services  
|--------------------------------------------------------------------------  
|  
| Services you want loaded into the container as singletons.  
|  
*/  
'services' => [
    '\App\Services\AuthService',
    '\App\Services\MyService',  
],
``` 

You can then access the service instance using `tr_service('MyService')`. If your service is outside the `App\Services` namespace, use the `tr_container()` function and pass the full class name.

## Application DI Container

Register a class to the DI Container.

```php
// singleton and alias are both optional arguments
$singleton = true;
$alias = 'my.class';

\TypeRocket\Core\Injector::register(\MyNamespace\MyClass::class, function () {
    $api_key = 'sdfjd83mvbfy92kf';

    return new \MyNamespace\MyClass($api_key);
}, $singleton, $alias);
```

Get an instance from the DI container.

```php
// by class name
$obj = \TypeRocket\Core\Injector::resolve(\MyNamespace\MyClass::class);

// by alias
$obj = \TypeRocket\Core\Injector::resolve('my.class');
```

Using helper.

```php
$obj = tr_container(\MyNamespace\MyClass::class)
```


## IoC Resolver

The inversion of control resolver automatically tries to instantiate a class with its dependencies. The `Resolver` object will look at a class' constructor and automatically inject its dependencies.

## Resolving

The `Resolver` classes method `resolve()` take one argument:

1. `$class` - The name of the class.

The `resolve()` method will check the class' constructor and try to resolve the dependencies by looking at the type hints in the constructor. If a constructor argument has a default value and no type-hint the default value will be used.

```php
// Classes
class Cheese {
    public $type;

    function __construct( $type = 'cheddar' ) {
        $this->type = $type;
    }
}

class Pizza {
    public $cheese;

    function __construct( \Cheese $cheese ) {
        $this->cheese = $cheese;
    }
}

// Resolver
$resolver = new \TypeRocket\Core\Resolver();
$pizza = $resolver->resolve('Pizza');
echo $pizza->cheese->type;
```

Will output,

```
cheddar
```

*Note: If one of the classes is located in the DI Container then the DI Container will be used for resolution of that class.*