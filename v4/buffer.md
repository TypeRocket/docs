Title: Buffer
Description: Buffer output and save it for later use.

---

## Buffering

Buffering allows you to capture printed or echoed data instead of outputting it right away.

## Basic Buffer

To start buffering use the `startBuffer()` method. Then, to stop the buffer and get the captured output use the `getCurrent()` method.

```php
echo 'First ';
$buffer = tr_buffer()->startBuffer();
echo 'Middle';
$middle = $buffer->getCurrent();
echo 'Last ';
echo $middle;
```

Will output,

```php
Top Last Middle
```

## Buffer Indexing

You can also index output to save chunks with the `indexBuffer()` method. You can then access the indexed buffer with the `getBuffer()` method.

```php
$buffer = tr_buffer();

$buffer->startBuffer();
echo '<li>One</li>';
$buffer->indexBuffer('one');

$buffer->startBuffer();
echo '<p>Two</p>';
$buffer->indexBuffer('two');

echo '<ul>';
echo $buffer->getBuffer('one');
echo '</ul>';

echo $buffer->getBuffer('two');
```

Will output,

```php
<ul><li>One</li></ul><p>Two</p>
```