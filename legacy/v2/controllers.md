Title: Controllers
Description: Controllers define how a resource is created, updated, read or deleted.

---

TypeRocket controllers are used to define what happens when a resource is created, updated, read or deleted.

## Creating a controller

To create a controller class create a file with the format `{resource}Controller`. For example, if you are creating a controller for a resource of `carts` the controller name would be `CartsController`.

You should locate your controller classes in `typerocket/app/Controllers/CartsController.php` and use the namespace `TypeRocket\Controllers`.

TypeRocket Controllers have two required method implementations: `update` and `create`.

```php
<?php // typerocket/app/Controllers/CartsController.php
namespace TypeRocket\Controllers

class CartsController extends Controller
{

    public function update( $id = null )
    {
    }

    public function create()
    {
    }

    public function read()
    {
    }

    public function delete()
    {
    }

}
```

## Post Type Controllers

If the new resource you are creating is a post type all you need to do is extend the `PostTypesController` class. Maybe `books` is your post type's resource.

```php
<?php // typerocket/app/Controllers/BooksController.php
namespace TypeRocket\Controllers;

class BooksController extends PostTypesController
{
}
```

Post type controllers only need to be defined and do not have any method implementations that need to be created although you are free to do so.