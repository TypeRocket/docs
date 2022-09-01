Title: Utilities
Description: Utility functions and tools.
Quick Links Depth: 3
Quick Links Columns: 3

---

## Array

### Arr::divide

The `Arr::divide()` method splits keys and values from the given array into divided parts:

```php
use \TypeRocket\Utility\Arr;

$array = ['one' => 1, 'two' => 2];
 
Arr::divide($array);
// [ ['one', 'two'],[1,2] ]
```

### Arr::filterNull

The `Arr::filterNull()` method removes all `null` values from the given array:

```php
use \TypeRocket\Utility\Arr;

$array = [0, null];
 
Arr::filterNull($array);
// [0 => 0]
```

### Arr::format

The `Arr::format()` method applies a callback to an arrays a values using dot notation. The returned value is the endpoint of the dot notation lookup. The array is mutated by reference.

```php
use \TypeRocket\Utility\Arr;

$array = [ 'name' => 'kevin'];
 
$location = Arr::format('name', $array, function($value) {
    return ucfirst($value)
});
// 'Kevin'  - $location
// [ 'name' => 'Kevin']  - $array
```

You can also format multiple values using the wild dot notation selector. The returned value will be `null`. The array is mutated by reference.

```php
use \TypeRocket\Utility\Arr;

$array = [ 
    'people' => [
        [ 'name' => 'kevin'],
        [ 'name' => 'jim']
    ]
];
 
$location = Arr::format('people.*.name', $array, function($value) {
    return ucfirst($value)
});
// null  - $location
// [ 'people' => [ [ 'name' => 'Kevin'], [ 'name' => 'Jim'] ] ]  - $array
```

### Arr::get

The `Arr::get()` method retrieves a value from a deeply nested array using dot notation:

```php
use \TypeRocket\Utility\Arr;

$array = ['products' => ['mic' => ['price' => 99]];
 
$price = Arr::get($array, 'products.mic.price');
// 99
```

The `Arr::get()` method also takes a default value, which will be returned if the specified key is **not present** in the array:

```php
use \TypeRocket\Utility\Arr;

$array = ['product' => ['name' => null, 'price' => 99]];
 
$price = Arr::get($array, 'product.tax', 10);
// 10

$price = Arr::get($array, 'product.name', 10);
// null
```

### Arr::has

The `Arr::has()` method checks if a given item exists in an array using dot notation:

```php
use \TypeRocket\Utility\Arr;

$array = ['product' => ['name' => null, 'price' => 99]];
 
$price = Arr::has($array, 'product.name');
// true

$price = Arr::has($array, 'product.tax');
// false
```

### Arr::indexBy

The `Arr::indexBy()` method indexes a sequential array by a key when all keyed values are unique:

```php
use \TypeRocket\Utility\Arr;

$array = [
    ['id' => 1, 'name' => 'Kevin'],
    ['id' => 2, 'name' => 'Matt'],
];
 
Arr::indexBy('id', $array);
// [
//   '1' => ['id' => 1, 'name' => 'Kevin'],
//   '2' => ['id' => 2, 'name' => 'Matt']
// ]
```

### Arr::isEmptyArray

The `Arr::isEmptyArray()` method checks if array has no values:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isEmptyArray([]);
// true

Arr::isEmptyArray(['']);
// false
```

### Arr::isSequential

The `Arr::isSequential()` method returns `true` if the given array's keys are sequential integers beginning from zero:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isSequential([0 => 'zero', 1 => 'one']);
// true

Arr::isSequential([1,2,3]);
// true
```

### Arr::isAccessible

The `Arr::isAccessible` method determines if the given value is array accessible:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isAccessible(['name' => 'Kevin']);
// true

Arr::isAccessible(new \ArrayObject());
// true

Arr::isAccessible(123);
// false
```

### Arr::isAssociative

The `Arr::isAssociative()` method returns `true` if the given array is an associative array. An array is considered "associative" if it doesn't have sequential numerical keys beginning with zero:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isAssociative(['name' => 'Kevin']);
// true

Arr::isAssociative([1 => 'one']);
// true
```

### Arr::keysExist

The `Arr::keysExist()` method check if a given list of keys exist within an array:

```php
use \TypeRocket\Utility\Arr;

$array = ['name' => 'mic', 'price' => 99];
 
Arr::isAssociative(['name', 'price'], $array);
// true

Arr::isAssociative(['tax'], $array);
// false
```

### Arr::meld

The `Arr::meld()` method reduces a deeply nested array into a dot notation keyed flat array:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isEmptyArray(['id' => [10,20]]);
// ['id.0' => 10, 'id.1' => 20]
```

### Arr::meldExpand

The `Arr::meldExpand()` method expands a dot notation keyed flat array into a deeply nested array:

```php
use \TypeRocket\Utility\Arr;
 
Arr::isEmptyArray(['id.0' => 10, 'id.1' => 20]);
// ['id' => [10,20]]
```

### Arr::only

The `Arr::only()` method gets a list of specified keys only:

```php
use \TypeRocket\Utility\Arr;

$array = ['name' => 'Mic', 'price' => 99, 'orders' => 5];
 
Arr::only($array, ['name', 'price']);
// ['name' => 'Mic', 'price' => 99]
```

### Arr::partition

The `Arr::partition()` method spreads a sequential array's values semi-evenly across a number of groups.

```php
use \TypeRocket\Utility\Arr;

$array = [1,2,3,4,5];
 
Arr::partition($array, 2);
// [[1,2,3],[4,5]]

Arr::partition($array, 3);
// [[1,2],[3,4],[5]]
```

### Arr::pluck

The `Arr::pluck()` method plucks specific values of a given key from an array:

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

### Arr::reduceAllowedStr

The `Arr::reduceAllowedStr()` method builds a string of unique words from an array's keys based on their boolean values:

```php
use \TypeRocket\Utility\Arr;

$array = [
    'button button-primary' => true,
    'button' => true,
    'link link-external' => false,
];
 
Arr::reduceAllowedStr($array);
// 'button button-primary'
```

### Arr::replaceRecursivePreferNew

The `Arr::replaceRecursivePreferNew()` method works like `array_replace_recursive` but keeps the order of the new array and allows for setting stop break points to the replacement search. The method take three arguments:

1. `$current_array`: `array` - The current array to recursively replace.
2. `$new_array`: `array` - The new array to recursively replace the current values with.
3. `$stops`: `array` - List of dot notation merge stop points where `$new_array`, when present, will force override and drop `$current_array` values after that point.

```php
use \TypeRocket\Utility\Arr;

$current = [
    'section' => [
        'name' => 'Development',
        'meta' => [
            'id' => 789,
            'type' => 'array',
        ]
    ],
];

$new = [
    'section' => [
        'name' => 'Testing',
        'meta' => [
            'type' => 'string',
        ]
    ],
];
 
Arr::replaceRecursivePreferNew($current, $new, ['section.meta']);
// [
//   'section' => 
//   [
//     'name' => 'Testing',
//     'meta' => 
//     [
//       'type' => 'string',
//     ],
//   ],
// ]
```

### Arr::set

The `Arr::set()` method assigns a given value to an item that exists in an array using dot notation:

```php
use \TypeRocket\Utility\Arr;

$array = ['product' => ['name' => null, 'price' => 99]];
 
$price = Arr::set('product.name', $array, 'mic');
// ['product' => ['name' => 'mic', 'price' => 99]]
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

### Str::classNames

The `Str::classNames()` method constructs a list of classes for an HTML element. The method builds a string of unique words from an array's keys based on their boolean values:

```php
use \TypeRocket\Utility\Str;

$array = [
    'button' => true,
    'button-primary' => true,
    'button-large' => true,
    'button-disabled' => false,
];
 
Str::classNames($array);
// 'button button-primary button-large'
```

If the first argument is a string it will be appended to the result:  

```php
use \TypeRocket\Utility\Str;

$base = 'button';

$array = [
    'button-primary' => true,
    'button-large' => true,
    'button-disabled' => false,
];
 
Str::classNames($base, $array);
// 'button button-primary button-large'
```

Finally, if a third argument is provided, and all array values are false, it will be prepended to the base string as a fallback:

```php
use \TypeRocket\Utility\Str;

$base = 'button';

$array = [
    'button-primary' => false,
    'button-large' => false,
];

$fallback = 'button-disabled';
 
Str::classNames($base, $array, $fallback);
// 'button button-disabled'
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

### Str::makeWords

The `Str::makeWords()` method replaces a delimiter with a space, `' '`, in given string with the option to title case the result.

```php
use \TypeRocket\Utility\Str;

Str::makeWords('foo_bar', false); 
// foo bar

Str::makeWords('foo_bar', true); 
// Foo Bar

Str::makeWords('foo-bar', true, '-'); 
// Foo Bar
```

### Str::notBlank

The `Str::notBlank()` method checks if a value is `null` or an empty string.

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

### Str::pregMatchFindFirst

The `Str::pregMatchFindFirst()` method returns the first regex pattern to match a given string. However, the regex applies `#` vs `/` as the delimiters:

```php
use \TypeRocket\Utility\Str;

// Do not use delimiters at the
// start and end of patterns.
$patters = [
    '[a-z]+',
    'brother',
    'dad',
    'mom'
];

$subject = 'dad';

Str::pregMatchFindFirst($patters, $subject);
// '[a-z]+'
```

### Str::replaceFirst

The `Str::replaceFirst()` method replaces the first occurrence of a given value in the string:

```php
use \TypeRocket\Utility\Str;

$replace = 'mom';
$with = 'po';
$string = 'momster mom';

Str::replaceFirst($replace, $with, $string);
// 'poster mom'
```

### Str::replaceFirstRegex

The `Str::replaceFirstRegex()` method replaces only the first match in a given string that is escaped by default with `preg_quote()`:

```php
use \TypeRocket\Utility\Str;

$replace = 'mom';
$with = 'po';
$string = 'momster mom';

Str::replaceFirstRegex($replace, $with, $string);
// 'poster mom'
```

Further, you can disable regex escaping of the replacement argument to enable regex:

```php
use \TypeRocket\Utility\Str;

$replace = '/mo./';
$with = 'po';
$string = 'monster mo.';

Str::replaceFirstRegex($replace, $with, $string, false);
// 'poster mo.'
```

### Str::replaceLast

The `Str::replaceLast()` method replaces the last occurrence of a given value in the string:

```php
use \TypeRocket\Utility\Str;

$replace = 'mom';
$with = 'po';
$string = 'monster mom';

Str::replaceLast($replace, $with, $string);
// 'momster po'
```

### Str::splitAt

The `Str::splitAt()` method splits at a delimiter string into two parts:

```php
use \TypeRocket\Utility\Str;

Str::splitAt('foo.bar.baz');
// ['foo', 'bar.baz']

Str::splitAt('.', 'fooBar');
// ['fooBar', null]

Str::splitAt('.', 'fooBar', true);
// ['foo.bar', 'baz']
```

### Str::snake

The `Str::snake()` method converts a string to snake case.

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

### Helper::appNamespace

The `Helper::appNamespace()` method gets the registered app namespace from the `TYPEROCKET_APP_NAMESPACE` constant.

```php
use TypeRocket\Utility\Helper;

Helper::appNamespace();
// 'App'
```

### Helper::controllerClass

The `Helper::controllerClass()` method gets the registered app namespace controller name standard with the `TYPEROCKET_APP_NAMESPACE` constant and appends the string `'Controller'` if needed.

```php
use TypeRocket\Utility\Helper;

Helper::controllerClass('Page');
// '\App\Controllers\PageController'
````

### Helper::form

The `Helper::form()` method creates a [form instance](/docs/v5/forms/) from the config class registered under `app.class.form`.

```php
use TypeRocket\Utility\Helper;

Helper::form();
```

### Helper::hash

The `Helper::hash()` method generates a unique runtime hash as an `int`.

```php
use TypeRocket\Utility\Helper;

Helper::hash();
```

### Helper::modelClass

The `Helper::modelClass()` method gets the registered app namespace models folder with the `TYPEROCKET_APP_NAMESPACE` constant.

```php
use TypeRocket\Utility\Helper;

Helper::modelClass('Page');
// '\App\Models\Page'
```

### Helper::wordPressRootPath

The `Helper::wordPressRootPath()` method tries to find the location of WordPress even when it is used before `ABSPATH` is defined.

```php
use TypeRocket\Utility\Helper;

Helper::wordPressRootPath();
```

## Data

The `TypeRocket\Utility\Data` class provides helper functions.

### Data::cast

The `Data::cast()` method will cast a value to the given type if the value is compatible with the cast type.

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

The `Data::isJson()` method returns `true` if the given value is valid `json`:

```php
use TypeRocket\Utility\Data;

Data::isJson('{"name":"smith"}');
// true

Data::isJson('');
// false

Data::isJson('""');
// false
```

### Data::nil

The `Data::nil()` method allows for chaining a number of property lookups without throwing an error. It does not provide access to object functions.

```php
use TypeRocket\Utility\Data;

$value = new \stdClass;
$value->name = 'Kevin'

Data::nil($value)['name']->get();
// null

Data::nil($value)->name->get();
// 'Kevin'

Data::nil($value)['one']->two->three; 
// Nil  - object

Data::nil($value)->one->['two']->three; 
// Nil  - object

Data::nil($value)->one->['two']->three->get();
// null
```

### Data::walk

The `Data::isJson()` method used dot notation to look up a value by array keys and/or object properties.

```php
use TypeRocket\Utility\Data;

$array = ['developer' => ['name' => 'Kevin']];

Data::walk('developer.name', $array);
// 'Kevin'
```