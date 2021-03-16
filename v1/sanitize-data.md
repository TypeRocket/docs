Title: Sanitize Data
Description: Sanitize many data types like textarea, attribute, url and more.

---

## Sanitizing Data

Never trust user input. Input that is not malicious could be malformed and break your site. The `Sanitize` class can help protect your site from bad data.

## Textarea

Remove all blocked tags defined by WordPress like `<script>`.

```php
$filtered = \TypeRocket\Utility\Sanitize::textarea( $value );
```

## Raw

Do not filter the value.

```php
$filtered = \TypeRocket\Utility\Sanitize::raw( $value );
```

## Attribute 

Escape the value for use in an HTML attribute.

```php
$filtered = \TypeRocket\Utility\Sanitize::attribute( $value );
```

## URL

Escape the URL.

```php
$filtered = \TypeRocket\Utility\Sanitize::url( $value );
```

## SQL

Escape SQL for a query.

```php
$filtered = \TypeRocket\Utility\Sanitize::sql( $value );
```

## Plaintext

Remove all HTML tags.

```php
$filtered = \TypeRocket\Utility\Sanitize::plaintext( $value );
```

## Editor

Filter HTML tags based on user capabilities. For example, an administrator with the capability  `unfiltered_html` will be allowed to enter raw data. Other users will be restricted like when using the WordPress Editor.

```php
$filtered = \TypeRocket\Utility\Sanitize::editor( $value , $force_filter, $auto_p);
```

## Hex

Escape a hexadecimal value like `#FFFFFF`.

```php
$filtered = \TypeRocket\Utility\Sanitize::hex( $value );
```

## Underscore

Remove all special characters and replace spaces and dashes with underscores allowing only a single underscore after trimming whitespace form string and lower casing.

```php
$value = 'First Name';
echo \TypeRocket\Utility\Sanitize::underscore( $value );
```

Will output,

```php
first_name
```

## Dash

Remove all special characters and replace spaces and underscores with dashes allowing only a single dash after trimming whitespace form string and lower casing.

```php
$value = 'First Name';
echo \TypeRocket\Utility\Sanitize::dash( $value );
```

Will output,

```php
first-name
```