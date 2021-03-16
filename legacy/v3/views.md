Title: Views
Description: Views are returned by a controllers for the page display. 

---

## Making a View

To create a `View` use the `tr_view()` helper function. This function takes two arguments:

1. `$view` - The view to be returned. This uses dot notation.
2. `$data` - An array of values to be [extracted](http://php.net/manual/en/function.extract.php) in the view. 

```php
tr_view('books.index', ['data' => 'my data']);
```

Because a view uses dot notation it can return different templates based on context. These contexts are: "Admin" and "Front-end".

## Admin Views

Admin views are located under `resources/pages` by default. When a controller returns a view in the admin context it will return the view from the specified folder.

Take this example view, in the admin context it will return the `resources/pages/books/index.php` template file.

```php
tr_view('books.index', ['data' => 'my data']);
```

## Front-end Views

Front-end views are located under `resources/views` by default. When a controller returns a view in the front-end context it will return the view from the specified folder.

Take this example view, in the front-end context it will return the `resources/views/books/index.php` template file.

```php
tr_view('books.index', ['data' => 'my data']);
```

### View Page Title

To set a views page `<head>` `<title>` tag use the `setTitle()` method.

```php
tr_view('books.add')->setTitle('Add Book');
```

## Templating

When a view is loaded like `resources/views/books/index.php` and `resources/pages/books/index.php` the passes array will be unpacked using the php `extract()` function.

So, a view with `['data' => 'my data']` passed to it have a `$data` variable with the string `'my data'` in its template file.

```php
// index.php
echo '<p>' . $data . '</p>';
```

Will output,

```html
<p>my data</p>
```

  