Title: Downloads
Description: Send downloads from a controller.

---

!!

## Getting Started

The `TypeRocketPro\Http\Download` class lets you send a downloadable file as the response to a request.

## Basic Usage

```php
$download = TypeRocket\Pro\Http\Download::new(__DIR__ . '/file.txt');
$download->setName('my-download-file-name.txt');
$download->send();
```

## From Route

Send files using a custom route you can sue the `download()` method.

```php
tr_route()->get()->on('my-path', function() {
    return \TypeRocket\Pro\Http\Download::new(__DIR__ . '/file.txt');
});
```

## From Storage

Send files to a client from [storage](/docs/v6/storage/) using a custom route you can sue the `download()` method.

```php
tr_route()->get()->on('my-path', function() {
    return \TypeRocket\Pro\Utility\Storage::download('file.txt');
});
```