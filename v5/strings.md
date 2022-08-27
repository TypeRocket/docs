Title: Strings
Description: Access common string utilities without rolling your own.
Quick Link Depth: 1
---

## String

The functions of the `\TypeRocket\Utility\Str` class are all UTF8 enabled and can work with Unicode characters.

<div class="content-columns">

[Str::camelize](#section-string-camelize)
[Str::contains](#section-string-contains)
[Str::ends](#section-string-ends)
[Str::notBlank](#section-string-not-blank)
[Str::snake](#section-string-snake)
[Str::starts](#section-string-starts)
[Str::trimStart](#section-string-trim-start)
[Str::uppercaseWords](#section-string-uppercase-words)

</div>

### Camelize

Use the method `camelize()` to camel case a string.

```php
$name = 'hi_there';
$capitalize_first_char = true;
$bool = \TypeRocket\Utility\Str::camelize($name, '_', $capitalize_first_char);
// HiThere
```

### Contains

Use the method `contains()` to test if a string contains a value.

```php
$needle = 'are';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::contains($needle, $haystack);
// true
```

### Ends

Use the method `ends()` to test if a string ends with a value.

```php
$needle = 'ol.';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::ends($needle, $haystack);
// true
```

### Not Blank

Use the method `notBlank()` to check if a value is `null` or an empty string.

```php
\TypeRocket\Utility\Str::notBlank(''); // false
\TypeRocket\Utility\Str::notBlank(null); // false
\TypeRocket\Utility\Str::notBlank(0); // true
\TypeRocket\Utility\Str::notBlank(' '); // true
```

### Snake

Use the method `snake()` to convert a string to snake case.

```php
\TypeRocket\Utility\Str::notBlank('fooBar');
// foo_bar

\TypeRocket\Utility\Str::notBlank('foo bar');
// foo_bar
```

### Starts

Use the method `starts()` to test if a string starts with a value.  

```php
$needle = 'Name';
$haystack = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::starts($needle, $haystack);
// true
```

### Trim Start

Use the method `trimStart()` to trim the start of a string.

```php
$name = 'hi_there';
\TypeRocket\Utility\Str::trimStart($name, 'hi_');
// there
```

### Uppercase Words

Use the method `uppercaseWords()` to apply `MB_CASE_TITLE` to a string.

```php
\TypeRocket\Utility\Str::uppercaseWords('hi_there');
// Hi_There
```