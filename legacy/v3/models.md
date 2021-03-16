Title: Models
Description: Manage your database tables and models easily.

---

## About Models

Models are object representations of database tables and how you want them to behave. For example, if you have a table called `wp_docs` with the columns `id` (primary), `version`, `title`, `json`, `created_at`, `modified_at`, and `slug` you can create a model to access and manipulate it.

## Making a Model

To create a model use the galaxy command `make:model`. When making a model with the CLI it can use one of three directives:

1. `base` - A model that represents any table in the database.
2. `post` - A model that represents a post type.
3. `term` - A model that represents a taxonomy term.

Both `post` and `term` are special and should be used with care. In many cases it is best to use the WordPress functions to deal with posts and terms so the hooks of other plugins are fired as expected.

The `base` directive is the most useful and is for making a model that is connected to custom tables.

### Command

Here is the syntax for making a model

```bash
make:model [-c|--controller] [--] <directive> <name> [<id>]
```

### Doc Example

To create a `Doc` model class, representing the table from the about section, run the the command: 

```bash
php galaxy make:model base Doc
```

Here is the class it creates:

```php
<?php
namespace App\Models;

use App\Results\Docs;
use TypeRocket\Models\Model;

class Doc extends Model
{
    protected $resource = 'docs';
}
```

## Model Class Template Code

There are three model classes you can work with: `Model`, `WPPost`, and `WPTerm`. It is best to use the Galaxy CLI to produce the template code for each of these types. However, you may want to make your models manually.

When making your models manually locate them in the `app/Models` directory.

### Base Model

```bash
php galaxy make:model base Doc
```

Will produce `app/Models/Doc.php`,

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Doc extends Model
{
    protected $resource = 'docs';
}
```

### Post Model

```bash
php galaxy make:model post Doc
```

Will produce `app/Models/Doc.php`,

```php
<?php
namespace App\Models;

use \TypeRocket\Models\WPPost;

class Doc extends WPPost
{
    protected $postType = 'doc';
}
```

### Term Model

```bash
php galaxy make:model term Version
```

Will produce `app/Models/Version.php`,

```php
<?php
namespace App\Models;

use \TypeRocket\Models\WPTerm;

class Version extends WPTerm
{
    protected $taxonomy = 'version';
}
```

## Basic Examples

You can query the database and retrieve the model you want. When a list of multiple items is returned a `Results` collection is returned.

```php
$docs = (new Doc())->where('version', 'v3')->findAll()->get();
```

You can also get a single item where the version is not `v3`.

```php
$doc = (new Doc())->where('version', '!=' ,'v3')->first();
```

And, where the ID is greater than 10.

```php
$doc = new Doc;
$doc->where('version', '!=' ,'v3')
$doc->where('id', '>' , 10);
$doc->first();
```

Or, a single `Doc` by ID.

```php
$doc = (new Doc())->findById(1);
```

## Fillable

To make limit what fields can be saved without be explicitly set use the `$fillable` property

```php
class Doc extends Model
{
    // ...

    protected $fillable = [
        'title',
        'json',
        'version',
        'slug'
    ];
}
```

## Guard

To make restrict a set of fields that can not be saved without be explicitly set use the `$guard` property

```php
class Doc extends Model
{
    // ...

    protected $guard = [
        'id',
        'created_at',
        'modified_at'
    ];
}
```

## Cast

To cast a properties value when it is being accessed use the `$cast` property.

```php
class Doc extends Model
{
    // ...

    protected $cast = [
        'id' => 'int', // cast to integer
        'json' => 'array', // cast a json string to an array
    ];
}
```

## Format

To format a models values before it is saved use the `$format` property. Format can be used to sanitize data being saved to the database.

```php
class Doc extends Model
{
    // ...

    protected $format = [
        'slug' => 'sanitize_title',
        'version' => 'static::sanitizeVersion'
    ];

    public static function sanitizeVersion( $value ) {
        return 'v' . filter_var($value, FILTER_SANITIZE_NUMBER_INT);
    }
}
```

## Query Engine

To create a model for manipulating tables rows create a new instance of the `Model` you want to work with.

```php
$doc = new Doc;
```

## Finding

There are four find methods models can use: `findById`, `findAll`, `findOrDie`, and `findFirstWhereOrDie`.

To get one item by its primary column ID: 

```php
$doc->findById(1);
```

To get a list of all records: 

```php
$doc->findAll()->get();
```

To get a list of all records with specific IDs: 

```php
$doc->findAll([1,2,3])->get();
```

To get one record by its primary column ID or die with `wp_die`: 

```php
$doc->findOrDie(1);
```

To get the first record: 

```php
$doc->first();
```

To get the first record by where the version is `v3` or die with `wp_die`:

```php
$doc->findFirstWhereOrDie('version', 'v3');
```

## Get

The `get()` method queries the database. If more than one result is queried a [results collection](/docs.v3.results-collection/) is returned.

```php
$result = $doc->select('id')->take(10)->get();
```

## Get Properties

The `getProperties()` method returns all the properties, also called fields, of a model.

```php
$fields = (new \App\Models\Post())->first()->getProperties();
```

## Where

There are two where method for models `where` and `orWhere`. These methods can be chained any way you like.

```php
$doc->where('version', '!=' ,'v3')->orWhere('id', 10);
$doc->first();
```

And,

```php
$doc->where('version', '!=' ,'v3')->where('id', 10);
$doc->first();
```

## Take: Limit and Offset

To take a certain scope of items use the `take()` method. This method takes two arguments:

1. `$limit` - The number of items to take.
1. `$offset` - The number of items to offset.

```php
$result = $doc->findAll()->take(10, 10)->get();
```

## Order By

You can set the ordering of results with `orderBy()`. this method takes two arguments:

1. `$column` - The column to order by, primary key is the default.
2. `$direction` - The direction or the ordering `ASC` (default) and `DESC`.

```php
$result = $doc->findAll()->orderBy('version', 'DESC')->get();
```

## Date Time

In some cases you will want to get the current date time for the MySQL database. You can use the `getDateTime()` method to access the current time in MySQL format.

```php
$time = $doc->getDateTime();
```

## Create

You can save any property explicitly set. Explicitly set properties ignore `$guard` and `$fillable`. To create a new row instance a model and explicitly set the values of the columns.


```php
$doc = new Doc;
$doc->title = 'My Title';
$doc->json = ['key'=> 'value'];
$doc->version = 'v3';
$doc->slug = $doc->title;
$doc->created_at = $doc->getDateTime();
$doc->modified_at = $doc->created_at;
$doc->create();
```


## Update

If your query has one result like with `findById()` you can save any property explicitly set.

```php
$doc = (new Doc)->findById(1);
$doc->json = ['key'=> 'new value'];
$doc->modified_at = $doc->created_at;
$doc->update();
```

## Save

The `save()` method will detect if a models primary key is set. If the key is set it will run the `update()` method and if not it will run the `create()` method.

## Delete

If your query has one result like with `findById()` you can delete it.

```php
$doc = (new Doc)->findById(1);
$doc->delete();
```

You can also bulk delete items.

```php
(new Doc)->delete([1,2,3]);
```

## Count

You can run the `count()` method on a model to get the number of items.

```php
$number = $doc->findAll()->count();
```

## Select
 
You can use the `select()` method get return specific columns only.

```php
$doc = (new Doc)->select('id', 'title')->first();
```

## Get ID and Set Primary Key 

The ID column is not always `id`. To access the primary key column use the `getID()` method. For example, in a model called `Member` the primary key might be the users email address.

```php
<?php
namespace App\Models;

use App\Results\Docs;
use TypeRocket\Models\Model;

class Member extends Model
{
    protected $resource = 'members';
    protected $idColumn = 'email';
}
```

Then,

```php
$email = (new Member)->first()->getID();
```

## Router Injection

If you are using custom routes and using controller injection you can set the automatic router injection column to something other than the primary ID by defining the `getRouterInjectionColumn()` method in the model class.

```php
class Doc extends Model
{
    // ...

    public function getRouterInjectionColumn()
    {
        return 'slug';
    }
}
```

For the custom route,

```php
// routes.php
tr_route('/docs/{slug}/', 'slug@Doc');
```

Using the `DocController` method `slug`,

```php
<?php
namespace App\Controllers;

use TypeRocket\Controllers\Controller;

class DocController extends Controller
{
    function slug( \App\Doc $doc, $slug ) {
        return tr_view('docs.view', compact('doc'))->setTitle($doc->title);
    }
}
```

## Relationships

If you want to assign relationships to models [view the relationships documentation](/docs/v3/relationships/).