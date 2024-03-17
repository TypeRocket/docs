Title: Models
Description: Manage your database tables and models easily.

---

## Before Starting

In most cases, your models will work as you intend. However, it is important to note the ORM is designed to work with WordPress as best it can while at the same time feeling like a true ORM.

Models that extend `WPPost`, `WPTerm`, `WPUser`, and `WPComment` are different from the base model class. These models use the core WordPress functions to `create`, `update`, and `delete`. This is by design to make the ORM more compatible with WordPress itself. So, when taking any of these actions, keep in mind your results may not be what you expect. When running queries on these models everything should work as expected.

Also, `WPOption` is not designed to work like a true model. It is designed to act as a key value store model. It does not pull records like you may expect. When running queries on this model you may get odd results.

## About Models

Models are object representations of database tables and how you want them to behave. For example, if you have a table called `wp_docs` with the columns `id` (primary), `version`, `title`, `json`, `created_at`, `modified_at`, and `slug` you can create a model to access and manipulate it.

## Making a Model

To create a model, use the galaxy command `make:model`. When making a model with the CLI, it can use one of three directives:

1. `base` - A model that represents any table in the database.
2. `post` - A model that represents a post type.
3. `term` - A model that represents a taxonomy term.

Both `post` and `term` are unique and should be used with care. In many cases, it is best to use the WordPress functions to deal with posts and terms, so the hooks of other plugins are fired as expected.

The `base` directive is the most useful and is for making a model that is connected to custom tables.

! **Note**: The Galaxy command-line tool is not available if you are using the official TypeRocket Framework 4 WordPress plugin.

### Command

Here is the syntax for making a model

```bash
make:model [-c|--controller] [--] <directive> <name> [<id>]
```

### Doc Example

To create a `Doc` model class, representing the table from the about section, run the command: 

```bash
php galaxy make:model base Doc
```

Here is the class it creates:

```php
<?php
namespace App\Models;

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
$docs = Doc::new()->where('version', 'v1')->find();
```

You can also get a single item where the version is not `v1`.

```php
$doc = Doc::new()->where('version', '!=' ,'v1')->first();
```

And, where the ID is greater than 10.

```php
$doc = new Doc;
$doc->where('version', '!=' ,'v1');
$doc->where('id', '>' , 10);
$doc->first();
```

Or, a single `Doc` by ID.

```php
$doc = Doc::new()->find(1);
```

## Fillable

To limit the fields that can be saved without being explicitly set, use the `$fillable` property.

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

Now, when a `Doc` is saved using `save()` only the fillable fields will be saved.

```php
$fields = [
    'title' => 'Setting Fillable Model Fields',
    'id' => 1,
    'content' => 'When saving fields to a model use fillable to restrict mass/bulk saving.',
];

// Only `title` from fields is saved
$doc->save($fields);

// Only `title` from fields is saved, and the
// explicitly set `content` is saved.  
$doc->content = $fields['content'];
$doc->save($fields);
```

## Guard

To restrict a set of fields that can not be saved without be explicitly set use the `$guard` property. In this example, `is`, `created_at`, and `modified_at` can not be saved using
the mass `save()` method.

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

Now,

```php
$fields = [
    'title' => 'Setting Guard Model Fields',
    'created_at' => 2021,
    'modified_at' => 2021,
];

// Only `title` from fields is saved
$doc->save($fields);

// Only `title` from fields is saved, and the
// explicitly set `modified_at` is saved.
$doc->modified_at = $doc->getDateTime();
$doc->save($fields);
```

## Cast

To cast a property value when it is accessed, use the `$cast` property.

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

To format a model's values before it is saved, use the `$format` property. Formatting can be used to sanitize data being saved to the database.

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

## Model Instance

To create a new model instance of the `Model` you want to work with.
```php
$doc = Doc::new();
```

## Finding

There are nine find methods models can use: `findById`, `findAll`, `find`, `first`, `findOrNew`, `findOrCreate`, `findOrDie`,  `findFirstWhereOrNew`, and `findFirstWhereOrDie`.

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

A shorthand for `findById` and `findAll`: 

```php
$doc->find([1,2,3]);
$doc->find(1);
```

To get one record by its primary column ID or get a new Model: 

```php
$doc->findOrNew(1);
```

To get one record by its primary column ID or create a new record: 

```php
$values = [
    'title' => 'Draft',
    'version' => 'v1',
];

$doc->findOrCreate(1, $values);
```

To get one record by its primary column ID or die with `wp_die`: 

```php
$doc->findOrDie(1);
```

To get the first record: 

```php
$doc->first();
```

To get the first record by where the version is `v1` or die with `wp_die`:

```php
$doc->findFirstWhereOrDie('version', 'v1');
```

To get the first record by where the version is `v1` or get new model:

```php
$doc->findFirstWhereOrNew('version', 'v1');
```

## Get

The `get()` method queries the database. If more than one result is queried a [results collection](/docs/v5/results-collection/) is returned.

```php
$result = $doc->select('id')->take(10)->get();
```

## Get Properties

The `getProperties()` method returns all the properties, also called fields, of a model.

```php
$fields = (new \App\Models\Post())->first()->getProperties();
```

## Has Properties

```php
(new \App\Models\Post())->hasProperties();
```

## Where

There are two `where` methods for models `where` and `orWhere`. These methods can be chained in any way you like.

```php
$doc->where('version', '!=' ,'v1')->orWhere('id', 10);
$doc->first();
```

And,

```php
$doc->where('version', '!=' ,'v1')->where('id', 10);
$doc->first();
```

### Grouped Where

You can also group where clauses.

```php
$where = [
    [
        'column' => 'ID',
        'operator' => '=',
        'value' => 1
    ],
    'OR',
    [
        'column' => 'ID',
        'operator' => '=',
        'value' => '2'
    ]
];

$doc->where($where);
```

### Where Meta

Models that have meta tables (`WPPost`, `WPUser`, `WPComment`, and `WPTerm`) can use the method `whereMeta()`. `whereMeta()` allows you to query the meta table of the model by the `meta_key` and `meta_value`.

```php
// short syntax for where feature meta value is not null
(new Post)->whereMeta('feature');
```

```php
// grouped meta query
(new Post)
    ->whereMeta([
        [
            'column' => 'feature',
            'operator' => '!=',
            'value' => null
        ],
        'AND',
        [
            'column' => 'video_url',
            'operator' => '!=',
            'value' => null
        ]
    ])
    ->get();
```

```php
// multiple meta queries
(new Post)
    ->whereMeta('video_url', 'like', 'https://facebook.com%')
    ->whereMeta('video_url', 'like', 'https://youtube.com%', 'OR')
    ->get();
```

## Take: Limit and Offset

To take a specific number of items, use the `take()` method. This method takes two arguments:

1. `$limit` - The number of items to take.
1. `$offset` - The number of items to offset.

```php
$result = $doc->findAll()->take(10, 10)->get();
```

## Order By

You can set the ordering of results with `orderBy()`. this method takes two arguments:

1. `$column` - The column to order by, the primary key is the default.
2. `$direction` - The direction or the ordering `ASC` (default) and `DESC`.

```php
$result = $doc->findAll()->orderBy('version', 'DESC')->get();
```

## Date Time

In some cases, you will want to get the current date-time for the MySQL database. You can use the `getDateTime()` method to access the current time in MySQL format.

```php
$time = $doc->getDateTime();
```

## Create

You can save any property explicitly set. Explicitly set properties ignore `$guard` and `$fillable`. To create a new row instance a model and explicitly set the values of the columns.


```php
$doc = Doc::new();
$doc->title = 'My Title';
$doc->json = ['key'=> 'value'];
$doc->version = 'v1';
$doc->slug = $doc->title;
$doc->created_at = $doc->getDateTime();
$doc->modified_at = $doc->created_at;
$doc->create();
```


## Update

If your query has one result like with `findById()`, you can save any property explicitly set.

```php
$doc = Doc::new()->findById(1);
$doc->json = ['key'=> 'new value'];
$doc->modified_at = $doc->created_at;
$doc->update();
```

## Save

The `save()` method will detect if a model's primary key is set. If the key is set, it will run the `update()` method, and if not it will run the `create()` method.

## Delete

If your query has one result like with `findById()`, you can delete it.

```php
$doc = Doc::new()->findById(1);
$doc->delete();
```

You can also bulk delete items via models you create that extend the base `Model` class. Models that extend `WPPost`, `WPTerm`, `WPUser`, `WPComment`, `WPOption` do not have the ability to bulk delete.

```php
Doc::new()->delete([1,2,3]);
```

## Select
 
You can use the `select()` method to return specific columns only.

```php
$doc = Doc::new()->select('id', 'title')->first();
```

## Functions

Every model object has the methods: `avg()`, `count()`, `max()`, `min()`, and `sum()`. Each of these functions accepts a single column name as a `string` or an instance of `\TypeRocket\Database\SqlRaw`.

```php
use \TypeRocket\Database\SqlRaw;

// Single column
$doc->sum('id');

// Expression
$doc->sum(SqlRaw::new('`id` * `id`'));
```

### Avg

The `avg()` method gets the average calculated value:

```php
$number = $doc->avg('id');
```

### Count

The `count()` method gets the count:

```php
$number = $doc->count();
```

### Max

The `max()` method gets the max calculated value:

```php
$number = $doc->max('id');
```

### Min

The `min()` method gets the min calculated value:

```php
$number = $doc->min('id');
```

### Sum

The `sum()` method gets the sum calculated value:

```php
$number = $doc->min('id');
```

## Get ID and Set Primary Key 

The ID column is not always `id`. To access the primary key column, use the `getID()` method. For example, in a model called `Member` the primary key might be the user's email address.

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

If you are using custom routes and using controller injection, you can set the automatic router injection column to something other than the primary ID by defining the `getDependencyInjectionKey()` method in the model class.
```php
class Doc extends Model
{
    // ...

    public function getDependencyInjectionKey()    {
        return 'slug';
    }
}
```

For the custom route,

```php
// routes.php
tr_route()->get()->on('docs/*', 'show@Doc');
```

Using the `DocController` method `slug`,

```php
<?php
namespace App\Controllers;

use TypeRocket\Controllers\Controller;

class DocController extends Controller
{
    function show( \App\Models\Doc $doc) {
        return tr_view('docs.view', compact('doc'))->setTitle($doc->title);
    }
}
```

## Relationships

If you want to assign relationships to models [view the relationships documentation](/docs/v5/relationships/).

## Accessors

You can now implement [Laravel-style accessors](https://laravel.com/docs/5.7/eloquent-mutators#accessors-and-mutators) on models.

```php
use \TypeRocket\Utility\Sanitize;

class Doc extends Model
{
    // ...

    public function getSlugProperty()
    {
        return Sanitize::dash($this->name);
    }
}
```

You now have access to a `slug` property on the `Doc` model.

```php
$doc = Doc::new();
echo $doc->slug;
```

To understand how this works, have a look at the `mutateProperty` function on the `Model` class:

```php
protected function mutateProperty($key, $value)
{
    return $this->{'get'.Str::camelize($key).'Property'}($value);
}
```

You can see that the bit between `get` and `Property` becomes the property name. Within the function, the property is Pascal case. When getting the value, the property is a snake case. So `$doc->getTheThingProperty()` becomes `$doc->the_thing`. This allows you to mutate values from the database on retrieval, cleans up your constructors and property declarations, and removes the need to keep calling getter methods with long names

## With: Eager Loading

You can eager load related models associated with a model using the `with()` method. With method takes a single `string` argument. The `string` value must be the method name used for the model's relationship.

For example, the `Post` model might have this relationship method:

```php
public function meta() {
    return $this->hasMany( WPPostMeta::class, 'post_id' );
}
```

You could eager load the associated `WPPostMeta` models using the `meta()` method by passing a string of `'meta'` to the `with()` method of the `Post` model before getting the results.

```php
(new \App\Models\Post)->with('meta')->findById(1);
(new \App\Models\Post)->published()->with('meta')->get();
```

You can also use dot syntax to eager load deeply into your models using the same pattern. For example, say we have a model named `MyCustomModel` and it has an `author` relationship and the `author` has a `posts` relationship. You can deeply eager load by passing the string `'author.posts'` to the `with()` method of the `MyCustomModel` model.

```php
(new \App\Models\MyCustomModel)->with('author.posts')->findById(1);
```

### Multiple Relationships

You can eager load multiple relationships by passing an array to the `with` method.

```php
(new \App\Models\MyCustomModel)
    ->with(['author.posts', 'author.videos', 'location'])
    ->findById(1);
```

### Scoping

You can scope eager loaded relationships by passing a callable to the `with` method using an array and the key as the relationship name.

```php
$pages = (new App\Models\Page)
    ->published()
    ->with(['meta' => function($query) {
        return $query->where('meta_key', 'feature');
    }])
    ->get();
```

Limiting the number of items eager loaded is not yet possible. If you need this feature, you can [create your own eager loading system](https://kevdees.com/raw-php-and-mysql-eager-loading/) until WordPress adds support for MySQL 8.

*Note: You can not limit the number of items on eager loaded relationships using `take` or another method. MySQL 8 has support for `ROW_NUMBER` which would make this possible, but WordPress does not support MySQL 8.*

### After Initial Query

Sometimes you will have an existing set of results and need to eager load after a query has already run. To eager load a relationship post query use the `load()` method. This method works on single models and results.  

```php
$pages = (new App\Models\Page)->published()->get();
$results = $pages->load('meta');
```

And, with scoping.

```php
$pages = (new App\Models\Page)->published()->get();

$results = $pages->load(['meta' => function($query) {
   return $query->where('meta_key', 'feature');
}]);
```

## toArray and toJson

Convert a model to an `array` or `json`.

```php
$model->toArray();
$model->toJson();
```

### Importing Relationships

When using eager loading, `toArray()` and `toJson` will also import the loaded relationships as a part of the return value.

```php
$machines = (new \App\Models\Manufacturer)
    ->with([
        'machines.machineType',
        'machines.industryType',
        'machines.machineConfiguration'
        ])
    ->findById(1)->get();

$machines->toArray();
$machines->toJson();
```

## Post Type Models

At times you will want to access the `WP_Post` object using a `WPPost` model without making an extra query to the database. You can now do this using the `wpPost()` method on any post type model.

Here is an advanced usage example on how this can be used to optimize your database performance.

```php
<?php
namespace App\Models;

use TypeRocket\Models\WPPost;

class Symbol extends WPPost
{
    protected $postType = 'post';

    public function getLinkProperty()
    {
        return get_the_permalink($this->wpPost());
    }

    public function getFeaturedImageProperty()
    {
        $id = !empty($this->meta->_thumbnail_id) ? $this->meta->_thumbnail_id : 0;

        return wp_get_attachment_image($id);
    }

}
```

On the Frontend:

```php
<?php
$symbol_model = new \App\Models\Symbol;

/**
 * Meta is eager loaded and post meta cache is updated.
 */
$symbols = $symbol_model->with('meta')->published()->whereMeta('highlight', '=', '1')->get();
?>

<?php if($symbols) : foreach ($symbols as $symbol) :
    /**
     * Using accessors the permalink and featured image are loaded
     * without overly querying the database.
     */
    ?>
    <a class="col-3" href="<?= $symbol->link; ?>">
        <?= $symbol->featured_image; ?>
    </a>
<?php endforeach; endif; ?>
```

## Pagination

```php
$per_page = 25;
$get_page = 1;
$pager = $model->paginate($per_page, $get_page);
```

Pagination results in object methods.

```php
$pager->getResults();
$pager->getCount();
$pager->getCurrentPage();
$pager->getNumberPerPage();
$pager->getNumberOfPages();
$pager->getNextPage();
$pager->getPreviousPage();
$pager->getLastPage();
$pager->getFirstPage();
$pager->getLinks();
$pager->toArray();
$pager->toJson();
```

```php
echo $pager;
// Outputs JSON format
```

## When

You can run the `when()` method on a model only do something when the value passed is not empty.

```php
$post = (new \App\Models\Post)->when(tr_request()->input('title'), function($model, $value) {
    $model->where('post_title', $value);
})->get();
```

## Advanced Recursive Replace

You can recursively replace current array data with new array data within a specific model column using the `Model::setArrayReplaceRecursiveKey()`. Using this method will not allow the current data to be deleted because it is a deep array replace function.

1. `$key`: `string` - The name of the column to recursively replace.
2. `$callback`: `callable` - A callback returning the new value just before the recursive replacement. The callback requires three arguments `$new`, `$current`, and `$key`.

```php
$model = new class extends Model 
{
    protected $properties = [
        'column' => [
            'first_name' => 'Kevin',
            'last_name' => 'Dees',
        ]
    ];
};

$new = [
    'column' => [
        'last_name' => 'Smith',
    ]
];

$model->setArrayReplaceRecursiveKey('column', function($new, $current, $key) {
    $new['last_name'] = 'Doe';
    return $new;
});
$model->save($new);
```

Result will be:

```php
[
    'column' => [
        'first_name' => 'Kevin',
        'last_name' => 'Doe',
    ]
]
```

Also, you can use `Model::setArrayReplaceRecursiveStops()` to mark stop points for the recursive replacements. This allows for the removal or total replacement of data after the stop point. `Model::setArrayReplaceRecursiveStops()` takes two arguments.

1. `$key`: `string` - The name of the column.
2. `$stops`: `array` - List of dot notation stop point against the current data.

```php
$model = new class extends Model 
{
    protected $properties = [
        'column' => [
            'first_name' => 'Kevin',
            'last_name' => 'Dees',
            'list' => [1,2,3],
            'group' => ['name' => ['a' => 'item'], 'age' => 12]
        ]
    ];
};

$new = [
    'column' => [
        'last_name' => 'Smith',
        'list' => [],
        'group' => ['name' => ['b' => 'item']]
    ]
];

$model->setArrayReplaceRecursiveKey('column');
$model->setArrayReplaceRecursiveStops('column', ['list', 'group.name']);
$model->save($new);
```

Result will be:

```php
[
    'column' => [
        'first_name' => 'Kevin',
        'last_name' => 'Smith',
        'list' => [],
        'group' => ['name' => ['b' => 'item'], 'age' => 12]
    ]
]
```

## Connections

If you need **multiple database connections** in WordPress, you can set up secondary `\wpdb` connections in TypeRocket by adding new drivers to the `database.drivers` config. For example, in your `config/database.php` file you will find the `alt` driver example with the following:

```php
'alt' => [
    'driver' => '\TypeRocket\Database\Connectors\CoreDatabaseConnector',
    'username' => typerocket_env('TYPEROCKET_ALT_DATABASE_USER'),
    'password' => typerocket_env('TYPEROCKET_ALT_DATABASE_PASSWORD'),
    'database' => typerocket_env('TYPEROCKET_ALT_DATABASE_DATABASE'),
    'host' => typerocket_env('TYPEROCKET_ALT_DATABASE_HOST'),
],
```

Here you can quickly set up the needed [configuration](https://typerocket.com/docs/v5/configuration/#section-defining-configuration-values) in your `wp-config.php` file. Then, use the driver to connect to the secondary location. From there, in your model classes, set the `connection` property to the database driver's config name of `alt`.

```php
use \TypeRocket\Utility\Sanitize;

class Doc extends Model
{
    protected $connection = 'alt';
    
    ...
}
```