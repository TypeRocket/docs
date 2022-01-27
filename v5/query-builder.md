Title: Query Builder
Description: Build SQL queries with JOIN, SELECT, INSERT, WHERE and others statements in a fluent way.

---

## Query Builder

To create an SQL statements create a new instance of the `Query` class. To make a custom query object instance the `\Typerocket\Database\Query` class.

```php
$query = new \Typerocket\Database\Query();
```

Or,

```php
$query = tr_query();
```

## Use Table

```php
$query = tr_query()->table('wp_posts');
```

## Get ID and Set Primary Key 

The ID column is not always `id`. You can set and get it via the `$idColumn` property accessor methods.

```php
$query = tr_query()->table('wp_posts')->setIdColumn('ID');
$idColumn = tr_query()->getIdColumn();
echo $idColumn;
```

Outputs,

```
ID
```

## Finding

*Note: for the sake of brevity, any time you see `$query` without a definition it equals `tr_query()->table('wp_posts')->setIdColumn('ID')`*

There are four find methods models can use: `findById`, `findAll`, `findOrDie`, and `findFirstWhereOrDie`. For the majority of the following section we will use the `wp_posts` table as the example and work with the following `$query` object:

```php
$query = tr_query()->table('wp_posts')->setIdColumn('ID');
```

To get one item by its primary column ID: 

```php
$query->findById(1);
```

To get a list of all items: 

```php
$query->findAll()->get();
```

To get a list of all items with specific IDs: 

```php
$query->findAll([1,2,3])->get();
```

To get one item by its primary column ID or die with `wp_die`: 

```php
$query->findOrDie(1);
```

To get the first item by where the version is `v4` or die with `wp_die`:

```php
$query->findFirstWhereOrDie('post_title', 'Hello world!');
```

## Get

The `get()` method queries the database for select calls.

```php
$result = $query->select('post_title', 'ID')->take(10)->get();
```

## Select
 
You can use the `select()` method get return specific columns only.

```php
$query->select('ID', 'title')->first();
```

Or, every column.

```php
$query->select()->first();
```

## Where

There are two where methods for models `where` and `orWhere`. These methods can be chained any way you like.

```php
$query->where('post_title', '!=' ,'Hello world!')->orWhere('id', 10);
$query->first();
```

And,

```php
$query->where('post_title', '!=' ,'Hello world!')->where('id', 2);
$query->first();
```

### Where In

```php
$query->where('id', 'IN', [1,2,3]);
```

### Where Not In

```php
$query->where('id', 'NOT IN', [1,2,3]);
```

### Raw Where

At times you need to inject raw information into your where statements. You can do this by using the `appendRawWhere()` method. For example:

```php
$query = new \TypeRocket\Database\Query();
        $query->table('wp_posts');
        $query->idColumn = 'ID';
        $query->run = false;
        $query
            ->appendRawWhere('AND', "post_status = 'publish'")
            ->appendRawWhere('OR', "(post_title = 'Hello World!' AND ID = 1)")
            ->update(['post_title' => 'My Title']);
```

Will output,

```
UPDATE wp_posts SET post_title='My Title' WHERE post_status = 'publish' OR (post_title = 'Hello World!' AND ID = 1)
```


## Take: Limit and Offset

To take a certain scope of items use the `take()` method. This method takes two arguments:

1. `$limit` - The number of items to take.
1. `$offset` - The number of items to offset.

```php
$result = $query->findAll()->take(10, 10)->get();
```

## Order By

You can set the ordering of results with `orderBy()`. this method takes two arguments:

1. `$column` - The column to order by, primary key is the default.
2. `$direction` - The direction or the ordering `ASC` (default) and `DESC`.

```php
$result = $query->findAll()->orderBy('post_title', 'DESC')->get();
```

## Group By

You can set the grouping of results with `groupBy()`. this method takes one argument:

1. `$column` - The column or columns to group by.

```php
$result = $query->findAll()->groupBy('ID')->get();
```

Or, multiple columns.

```php
$result = $query->findAll()->groupBy(['ID','name','email'])->get();
```

## Date Time

In some cases, you will want to get the current date-time for the MySQL database. You can use the `getDateTime()` method to access the current time in MySQL format.

```php
$time = $query->getDateTime();
```

## Create

You can create records like so.

```php
tr_query()->table('wp_postmeta')->setIdColumn('meta_id')->create([
    'post_id' => 1,
    'meta_key' => 'key',
    'meta_value' => 'value'
]);
```

## Update

You can update records like so.

```php
$data = [
    'post_id' => 1,
    'meta_key' => 'key',
    'meta_value' => 'value'
];

tr_query()->table('wp_postmeta')
          ->setIdColumn('meta_id')
          ->findById(1)
          ->update($data);
```

## Delete

If your query has one result like with `findById()` you can delete it.

```php
$query->findById(1)->delete();
```

You can also bulk delete items.

```php
$query->delete([1,2,3]);
```

## Distinct

To return only unique results use the `distinct()` method.

```php
$query->distinct()->get();
```

## Joins

Joins are powerful tools for merging tables. Here we join the `wp_posts` table with `wp_postmeta`. There are three join methods: `join()`, `leftJoin()`, and `rightJoin()`.

### Join

The `join()` method takes 5 arguments.

1. `$table` - The table to be queried.
2. `$column` - A column to join on.
3. `$arg1` - The second column to join on. Or, the operator (=, !=, >, <, ...).
4. `$arg2` (optional) - The second column and `$arg1` must be an operator.
5. `$type`(optional) - The type of join: `INNER`, `LEFT`, and `RIGHT`.

Here is an example join query.

```php
$query = tr_query()->table('wp_posts');
$query->setIdColumn('ID');
$query->select('wp_posts.post_title', 'wp_posts.ID', 'wp_postmeta.meta_key')
    ->distinct()
    ->join('wp_postmeta', 'wp_postmeta.post_id', '=', 'wp_posts.ID', 'INNER')
    ->where('wp_posts.ID', 1)
    ->get();
```

The shorthand with the same result,

```php
$query = tr_query()->table('wp_posts');
$query->setIdColumn('ID');
$query->select('wp_posts.post_title', 'wp_posts.ID', 'wp_postmeta.meta_key')
    ->distinct()
    ->join('wp_postmeta', 'wp_postmeta.post_id', 'wp_posts.ID')
    ->where('wp_posts.ID', 1)
    ->get();
```

### Left and right Joins

The `leftJoin()` and `rightJoin()` methods work in the same way as the `join()` method, except they do not have a fifth argument. Here is a left join in shorthand.

```php
$query = tr_query()->table('wp_posts');
$query->setIdColumn('ID');
$query->select('wp_posts.post_title', 'wp_posts.ID', 'wp_postmeta.meta_key')
    ->distinct()
    ->leftJoin('wp_postmeta', 'wp_postmeta.post_id', 'wp_posts.ID')
    ->where('wp_posts.ID', 1)
    ->get();
```

## Union

You can combine two tables' results as well with the `union()` method.

```php
$last = tr_query()->table('wp_posts')->setIdColumn('ID');
$last->select('post_title', 'ID')
              ->where('ID', 2);

$first = tr_query()->table('wp_posts')->setIdColumn('ID');
$posts = $first->select('post_title', 'ID')
              ->where('ID', 1)
              ->union($last) // union
              ->get();
```

## Functions

You can do some essential math function calls as well.

### Count

You can run the `count()` method on a model to get the number of items.

```php
$var = $query->count();
```

### Min

You can run the `min()` method to get the min of a column.

```php
$var = $query->min('ID');
```

### Max

You can run the `max()` method to get the max of a column.

```php
$var = $query->max('ID');
```

### Avg

You can run the `avg()` method to get the average of a column.

```php
$var = $query->avg('ID');
```

### Sum

You can run the `sum()` method to get the sum of a column.

```php
$var = $query->sum('ID');
```
