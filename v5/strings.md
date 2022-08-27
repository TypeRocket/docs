Title: Strings
Description: Access common string utilities without rolling your own.

---

## String

The functions of the `\TypeRocket\Utility\Str` class are all UTF8 enabled and can work with Unicode characters.

<div class="content-columns">

[Str::starts](#section-starts)
[Str::ends](#section-ends)
[Str::contains](#section-contains)
[Str::camelize](#section-camelize)
[Str::trimStart](#section-trim-start)

</div>

## Starts

Use the method `starts()` to test if a string starts with a value.  

```php
$needle = 'Name';
$subject = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::starts( $needle, $subject );
```

## Ends

Use the method `ends()` to test if a string ends with a value.  

```php
$needle = 'ol.';
$subject = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::ends( $needle, $subject );
```

## Contains

Use the method `contains()` to test if a string contains a value.  

```php
$needle = 'are';
$subject = 'Names are cool.';
$bool = \TypeRocket\Utility\Str::contains( $needle, $subject );
```

## Camelize

Use the method `camelize()` to camel case a string.  

```php
$name = 'hi_there';
$capitalize_first_char = true;
$bool = \TypeRocket\Utility\Str::camelize($name, '_', $capitalize_first_char);
```


## Trim Start

Use the method `trimStart()` to trim the start of a string.  

```php
$name = 'hi_there';
$bool = \TypeRocket\Utility\Str::trimStart($name, 'hi_');
```