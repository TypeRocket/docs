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
 
Arr::filterNull($array);
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
 
Arr::only($array, ['name', 'price']);
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
 
Arr::pluck($array, ['name']);
// ['John', 'Sally']

Arr::pluck($array, ['name', 'title']);
// [ ['name' => 'John', 'title' => 'Dev'], ['name' => 'Sally', 'title' => 'Dev'] ]
```

Further, you can index the result by a given key: 

```php
use \TypeRocket\Utility\Arr;

$array = [
    ['id' => 123, 'title' => 'Dev', 'name' => 'John'],
    ['id' => 789, 'title' => 'Dev', 'name' => 'Sally'],
]; 

Arr::pluck($array, ['name'], 'id');
// [123 => 'John', 789 => 'Sally']
```

## String

The functions of the `\TypeRocket\Utility\Str` class are all UTF8 enabled and can work with Unicode characters.

### Str::camelize

Use the method `Str::camelize()` to camel case a string.

```php
use \TypeRocket\Utility\Str;

$name = 'hi_there';
$capitalize_first_char = true;

Str::camelize($name, '_', $capitalize_first_char);
// 'HiThere'
```

### Str::contains

Use the method `Str::contains()` to test if a string contains a value.

```php
use \TypeRocket\Utility\Str;

Str::contains('it', 'Name it!');
// true
```

### Str::ends

Use the method `Str::ends()` to test if a string ends with a value.

```php
use \TypeRocket\Utility\Str;

Str::ends('it!', 'Names it!');
// true
```

### Str::explodeFromRight

Use the method `Str::explodeFromRight()` to test if a string contains a value.

```php
use \TypeRocket\Utility\Str;

Str::contains('it', 'Name it!');
// true
```

### Str::notBlank

Use the method `Str::notBlank()` to check if a value is `null` or an empty string.

```php
use \TypeRocket\Utility\Str;

Str::notBlank(''); 
// false

Str::notBlank(null); 
// false

Str::notBlank(0); 
// true

Str::notBlank(' ');
// true
```

### Str::snake

Use the method `Str::snake()` to convert a string to snake case.

```php
use \TypeRocket\Utility\Str;

Str::snake('fooBar');
// 'foo_bar'

Str::snake('foo bar');
// 'foo_bar'
```

### Str::starts

Use the method `Str::starts()` to test if a string starts with a value.

```php
use \TypeRocket\Utility\Str;

Str::starts('Name', 'Name it!');
// true
```

### Str::trimStart

Use the method `Str::trimStart()` to trim the start of a string.

```php
use \TypeRocket\Utility\Str;

Str::trimStart('hi_there', 'hi_');
// 'there'
```

### Str::uppercaseWords

Use the method `Str::uppercaseWords()` to apply `MB_CASE_TITLE` to a string.

```php
use \TypeRocket\Utility\Str;

Str::uppercaseWords('hi_there');
// 'Hi_There'
```

## tr_nils()

The `tr_nils()` function allows for chaining a number of property lookups without throwing an error. It does not provide access to object functions.

```php
$value = new \stdClass;

tr_nils($value)['one']->two->three; 
// returns Nil object

tr_nils($value)->one->two->three; 
// returns Nil object

tr_nils($value)->one->two->three->get();
// returns null or real value if exists
```

## File

The `\TypeRocket\Utility\File` utility class offers a number of advanced file management features. First, create a new `\TypeRocket\Utility\File` instance to work with. This is an instance only and no file is created, deleted, or updated at this point.

```php
use \TypeRocket\Utility\File;

$file = File::new($file_path);
```

### Append

Append content to an existing file.

```php
$file->append('more content');
```

### Create

Create a file with content.

```php
$file->create('content');
```

### Exists

Check if file exists.

```php
$file->exists();
```

### Last Modified

Get the last modified date as a Unix timestamp of a file or return `false`.

```php
$file->lastModified();
```

### Mime Type

Get the mine-type of an existing file.

```php
$file->mimeType();
```

### Read

Get the content of an existing file.

```php
$file->read();
```

### Read First Line

Read the first line of an existing file.

```php
$file->readFirstLine();
```

### Read First Characters To

Read the content for a specific length of characters and of an offset from an existing file.

```php
$file->readFirstCharactersTo($length, $offset);
```

### Remove

Remove an existing file.

```php
$file->remove();
```

### Replace

Replace an existing file with a new file containing content.

```php
$file->replace('new content');
```

### Size

Get the size of an existing file in bytes, or `false`.

```php
$file->size();
```

### Wrote

Has file been written to by this instance.

```php
$file->wrote();
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