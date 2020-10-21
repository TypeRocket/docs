Title: Models
Description: Models are used to define the data structure of your resources.

---

## About Models

TypeRocket models are used to define the data structure of your resources. 

To create a model class create a file with the format {resource}Model. For example, if you are creating a controller for a resource of carts the controller name would be `CartsModel`.

You should locate your model classes in `typerocket/app/Models/CartsModel.php` and use the namespace `TypeRocket\Models`.

```php
<?php // typerocket/app/Models/CartsModel.php
namespace TypeRocket\Models;

class CartsModel extends Model
{
}
```

## Post Type Models

If the new resource you are creating is a post type all you need to do is extend the `PostTypesModel` class. Maybe `books` is your post type's resource for `book`.

All you need to define is the post type name/ID with the protected `postType` property.

```php
<?php // typerocket/app/Models/BooksModel.php
namespace TypeRocket\Models;

class BooksModel extends PostTypesModel
{
     protected $postType = 'book';
}
```

## Fillable

By default TypeRocket will save all custom field fields to the database outside of guarded fields. If you want to make sure only certain custom fields can be saved to the database add the `fillable` property.

When the `fillable` property is defined TypeRocket will stop saving every field and only save the fields that are defined. 

```php
<?php // typerocket/app/Models/BooksModel.php
namespace TypeRocket\Models;

class BooksModel extends PostTypesModel
{
     protected $postType = 'book';

     protected $fillable = array(
        'book_cover',
        'isbn',
        'author_name'
     );

}
```

## Guard

If you want to make sure certain custom fields can't be saved to the database add the `guard` property. However, if a field is marked as fillable it will not be guarded.

```php
<?php // typerocket/app/Models/BooksModel.php
namespace TypeRocket\Models;

class BooksModel extends PostTypesModel
{
     protected $postType = 'book';

     protected $fillable = array(
        'book_cover',
        'isbn',
        'author_name'
     );

     protected $guard = array(
        'post_type',
        'id'
     );

}
```