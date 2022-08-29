Title: Utilities
Description: Utility functions and tools.
Quick Links Depth: 3
Quick Links Columns: 3

---

## Array

### Arr::filterNull

The `Arr::filterNull` method removes all `null` values from the given array:

```php
use \TypeRocket\Utility\Arr;

$array = [0, null];
 
$filtered = Arr::filterNull($array);
 
// [0 => 0]
```

### Arr::isEmptyArray

The `Arr::isEmptyArray` method checks if array has no values:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isEmptyArray([]);
 
// true

Arr::isEmptyArray(['']);
 
// false
```

### Arr::only

```php
use \TypeRocket\Utility\Arr;

$array = ['name' => 'Desk', 'price' => 100, 'orders' => 10];
 
$slice = Arr::only($array, ['name', 'price']);
 
// ['name' => 'Desk', 'price' => 100]
```

### Arr:pluck

The `Arr::pluck` method plucks specific values of a given key from an array:

```php
use \TypeRocket\Utility\Arr;

$array = [
    ['id' => 123, 'title' => 'Dev', 'name' => 'John'],
    ['id' => 789, 'title' => 'Dev', 'name' => 'Sally'],
];
 
$names = Arr::pluck($array, ['name']);
 
// ['John', 'Sally']

$names = Arr::pluck($array, ['name', 'title']);
 
// [ ['name' => 'John', 'title' => 'Dev'], ['name' => 'Sally', 'title' => 'Dev'] ]
```

Further, you can index the result by a given key: 

```php
use \TypeRocket\Utility\Arr;

$array = [
    ['id' => 123, 'title' => 'Dev', 'name' => 'John'],
    ['id' => 789, 'title' => 'Dev', 'name' => 'Sally'],
]; 
$names = Arr::pluck($array, ['name'], 'id');
 
// [123 => 'John', 789 => 'Sally']
```

## String

The functions of the `\TypeRocket\Utility\Str` class are all UTF8 enabled and can work with Unicode characters.

### Str::camelize

Use the method `camelize()` to camel case a string.

```php
$name = 'hi_there';
$capitalize_first_char = true;
$bool = \TypeRocket\Utility\Str::camelize($name, '_', $capitalize_first_char);
// HiThere
```

### Str::contains

Use the method `contains()` to test if a string contains a value.

```php
$needle = 'are';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::contains($needle, $haystack);
// true
```

### Str::ends

Use the method `ends()` to test if a string ends with a value.

```php
$needle = 'ol.';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::ends($needle, $haystack);
// true
```

### Str::notBlank

Use the method `notBlank()` to check if a value is `null` or an empty string.

```php
\TypeRocket\Utility\Str::notBlank(''); // false
\TypeRocket\Utility\Str::notBlank(null); // false
\TypeRocket\Utility\Str::notBlank(0); // true
\TypeRocket\Utility\Str::notBlank(' '); // true
```

### Str::snake

Use the method `snake()` to convert a string to snake case.

```php
\TypeRocket\Utility\Str::snake('fooBar');
// foo_bar

\TypeRocket\Utility\Str::snake('foo bar');
// foo_bar
```

### Str::starts

Use the method `starts()` to test if a string starts with a value.

```php
$needle = 'Name';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::starts($needle, $haystack);
// true
```

### Str::trimStart

Use the method `trimStart()` to trim the start of a string.

```php
$name = 'hi_there';
\TypeRocket\Utility\Str::trimStart($name, 'hi_');
// there
```

### Str::uppercaseWords

Use the method `uppercaseWords()` to apply `MB_CASE_TITLE` to a string.

```php
\TypeRocket\Utility\Str::uppercaseWords('hi_there');
// Hi_There
```

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

## File

The `\TypeRocket\Utility\File` utility class offers a number of advanced file management features.

```php
use \TypeRocket\Utility\File;

$file = File::new($file_path);
$file->create('content');
$file->append('more content');
$file->replace('new content');
$file->remove();
$file->exists();
$file->wrote();
$file->exists();
$file->read();
$file->readFirstLine();
$file->readFirstCharactersTo($length, $offset);
$file->mimeType();
$file->lastModified();
$file->size();
```

## Helper

The `TypeRocket\Utility\Helper` class includes a number of functions:

### Helper::wordPressRootPath

This function tries to find the location of WordPress even when it is used before `ABSPATH` is defined.

```php
TypeRocket\Utility\Helper::wordPressRootPath();
```

## Data

The `TypeRocket\Utility\Data` class provides helper functions.

### Data::cast

The `Data::cast` method will cast a value to the given type if the value is compatible with the cast type.

```php
use TypeRocket\Utility\Data;

Data::cast(100, 'str');
// '100'

Data::cast(0, 'bool');
// false

Data::cast(1, 'float');
// 1.0

Data::cast(1.0, 'int');
// 1

Data::cast(['name' => 'smith'], 'serial');
// a:1:{s:4:"name";s:5:"smith";}

Data::cast(['name' => 'smith'], 'json');
// {"name":"smith"}

Data::cast('{"name":"smith"}', 'array');
// ['name' => 'smith']

Data::cast(1, 'array');
// 1  - is not compatible with array cast
```

### Data:isJson

The `Data::cast` method returns `true` if the given value is valid `json`:

```php
use TypeRocket\Utility\Data;

Data::isJson('{"name":"smith"}');
// true

Data::isJson('');
// false

Data::isJson('""');
// false
```

### Data::walk

```php
use TypeRocket\Utility\Data;

$array = ['developer' => ['name' => 'Kevin']];

Data::walk('developer.name', $array);
// 'Kevin'
```