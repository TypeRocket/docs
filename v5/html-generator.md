Title: HTML Generator
Description: Generate HTML elements without complicated string manipulation.

---

## Make Custom Tag

```php
echo \TypeRocket\Html\Html::el('div', ['class' => 'container'], 'Inner Content');
```

Will output,

```html
<div class="container">Inner Content</div>
```

## Make Input

```php
echo \TypeRocket\Html\Html::input('text', 'field_name', 'the value');
```

Will output,

```html
<input type="text" name="field_name" value="the value" />
```

## Make Image

```php
echo \TypeRocket\Html\Html::img('http://example.com/image_src.png');
```

Will output,

```html
<img src="http://example.com/image_src.png" />
```

## Make Link

```php
echo \TypeRocket\Html\Html::a('Link Text', 'http://example.com/');
```

Will output,

```html
<a href="http://example.com/">Link Text</a>
```

## Make Form

```php
echo \TypeRocket\Html\Html::form('submit.php', 'POST');
```

Will output,

```html
<form action="submit.php" method="POST"></form>
```

## Dynamic Tags

All other tags that do not have a method natively defined can be generated using the tag name as the method name. This gives you a shorthand for the `el()`  method.

```php
echo \TypeRocket\Html\Html::el('div', ['class' => 'container'], 'Inner Content');

// As..

echo \TypeRocket\Html\Html::div(['class' => 'container'], 'Inner Content');
```

## Nesting

You can nest tags or content within other tags using `nest()`.

```php
$ul =  \TypeRocket\Html\Html::ul();
$tag->nest(\TypeRocket\Html\Html::li('Item 1'));
```