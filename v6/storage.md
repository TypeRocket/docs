Title: Storage
Description: Custom storage system.

---

!!

## Getting Started

You can use the `\TypeRocket\Pro\Utility\Storage` class to work with files from specific locations. The class makes it easy to work with files, like the `TypeRocket\Utility\File` class, with the benefit of working with files in external and internal locations.

## Configure

To configure the storage system to a specific storage driver see the `app/storage.php` config file. By default, TypeRocket uses its `storage` storage driver. There are four storage drivers included with TypeRocket:

- `storage`: Work with files relative to the TypeRocket `storage` folder.
- `uploads`: Work with files relative to the `wp-content/uploads` folder.
- `root`: Work with files relative to the `ABSPATH` folder.
- `stack`: Work with files with a list of drivers.

## Basic Usage

```php
\TypeRocket\Pro\Utility\Storage::create('file.txt', 'content');
\TypeRocket\Pro\Utility\Storage::append('file.txt', 'content');
\TypeRocket\Pro\Utility\Storage::replace('file.txt', 'content');
\TypeRocket\Pro\Utility\Storage::get('file.txt');
\TypeRocket\Pro\Utility\Storage::delete('file.txt');
\TypeRocket\Pro\Utility\Storage::exists('file.txt');
\TypeRocket\Pro\Utility\Storage::path('file.txt');
\TypeRocket\Pro\Utility\Storage::size('file.txt');
\TypeRocket\Pro\Utility\Storage::lastModified('file.txt');
```

## Switch Driver

If you do not want to use the default driver you can specify one.

```php
\TypeRocket\Pro\Utility\Storage::driver('uploads')->create('file.txt');
```

## Custom Stacks

You can log to a custom stack as well.

```php
\TypeRocket\Pro\Utility\Storage::stack(['uploads', 'storage'], 'create', 'file.txt', 'content');
```

## Downloads

[Send files to a client for a download](/docs/v6/downloads/) using a custom route you can sue the `download()` method.

```php
tr_route()->get()->on('my-path', function() {
    return \TypeRocket\Pro\Utility\Storage::download('file.txt');
});
```