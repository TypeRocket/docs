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

## File

The `\TypeRocket\Utility\File` utility class offers a number of advanced file management features.

```php
$file = \TypeRocket\Utility\File::new($file_path);
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

### Get The WordPress Directory

This function tries to find the location of WordPress even when it is used before `ABSPATH` is defined.

```php
TypeRocket\Utility\Helper::wordPressRootPath();
```

## Data

The `TypeRocket\Utility\Data` class provides helper functions.

### Walk

```php
$array = ['my' => ['key' => 'value']];

echo TypeRocket\Utility\Data::walk('my.key', $array);
// outputs: value
```

### Cast

```php
// returns string '100'
TypeRocket\Utility\Data::cast(100, 'str');

// returns false
TypeRocket\Utility\Data::cast(0, 'bool');

// returns serialized array
TypeRocket\Utility\Data::cast(['name' => 'smith'], 'serial');
```