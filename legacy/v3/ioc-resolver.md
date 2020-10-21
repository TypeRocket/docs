Title: IoC Resolver
Description: Inversion of control resolver automatically tries to instance a class with its dependancies.

---

## IoC Resolver

The inversion of control resolver automatically tries to instance a class with its dependancies. The `Resolver` object will look at a classes constructor and automatically inject its dependencies.

## Resolving

The `Resolver` classes method `resolve()` take one argument:

1. `$class` - The name of the class.

The `resolve()` method will check the classes constructor and try to resolve the dependencies by looking at the type hints in the constructor. If a constructor argument has a default value and no type hint the default value will be used.

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