Title: Utilities
Description: Utility functions and tools.

---

## tr_nils()

The `tr_nils()` function allows for chaining a number of property lookups without throwing an error. It does not provide access to object functions.

```php
$value = new \stdClass;

// returns Nil object
tr_nils($value)['one']->two->three; 

// returns Nil object
tr_nils($value)->one->two->three; 

// returns null or real value if exists
tr_nils($value)->one->two->three->get();
```

## tr_cast()

The `tr_cast($value, $type)` function will cast almost any value into another type or return `null`. Formats include:

- int
- float
- json
- serial
- str
- bool
- object

```php
// returns string '100'
tr_cast(100, 'str');

// returns false
tr_cast(0, 'bool');

// returns serialized array
tr_cast(['name' => 'smith'], 'serial');
```