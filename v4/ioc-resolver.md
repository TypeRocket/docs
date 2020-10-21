Title: DI Container
Description: Inversion of control resolver automatically tries to instance a class with its dependancies.

---

## Application DI Container

Register a class to the DI Container.

```php
// singlton and alias are both optional arguments
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