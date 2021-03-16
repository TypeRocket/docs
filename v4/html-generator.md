Title: HTML Generator
Description: Generate HTML elements without complicated string manipulation.

---

## Make Custom Tag

Making a tag is simple with the `Generator` class.

```php
$gen = new \TypeRocket\Html\Generator();
$gen->newElement('div', ['class' => 'container'], 'Inner Content');
echo $gen;
```

Will output,

```html
<div class="container">Inner Content</div>
```

## Make Input

```php
$gen = new \TypeRocket\Html\Generator();
$gen->newInput('text', 'field_name', 'the value');
echo $gen;
```

Will output,

```html
<input type="text" name="field_name" value="the value" />
```

## Make Image

```php
$gen = new \TypeRocket\Html\Generator();
$gen->newImage('http://example.com/image_src.png');
echo $gen;
```

Will output,

```html
<img src="http://example.com/image_src.png" />
```

## Make Link

```php
$gen = new \TypeRocket\Html\Generator();
$gen->newLink('Link Text', 'http://example.com/');
echo $gen;
```

Will output,

```html
<a href="http://example.com/">Link Text</a>
```

