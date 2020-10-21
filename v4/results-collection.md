Title: Results Collection
Description: Custom collections can power 

---

## Result Collections

When you are using a `Model` to retrieve a set or records a `Results` collection will be returned. A results collection will automatically convert any returned record to the corresponding model. This means that when you query a model and a list of records is returned all the records will be models instead of an array.

Take a looks at the example. Here we query for posts and they are all returned `Post` models. This will let us manipulate them quickly if we want to.

```php  
$post = new \App\Models\Post();
$posts = $post->findAll()->published()->get();

foreach( $posts as $post ) {
    var_dump( get_class($post) );
}
```

Will output,

```php
'App\Models\Post'
'App\Models\Post'
'App\Models\Post'
```

## Custom Collections

You can make your own custom collections as well. Take a look at creating a custom results collection called `Library`. In your `app` folder create a new directory named `Results` and add the following file.

```php
<?php
namespace App\Results;

use App\Models\Book;
use TypeRocket\Database\Results;

class Library extends Results
{
}
```

Then, in the `Book` model class define the collection you want returned instead of the default `Results` class.

```php
<?php
namespace App\Models;

use App\Results\Library;
use TypeRocket\Models\Model;

class Doc extends Model
{
    // ...

    protected $resultsClass = Library::class;
}
```