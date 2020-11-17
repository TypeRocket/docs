Title: Tables
Description: Tables a used for displaying data from models.

---

*Pro Only: This is a Pro only extension feature.*

## Tables

UI tables are used to list the records of a Model. Tables can be added to admin pages or anywhere else. When a table is created within the context of a resource admin view all the required settings are detected and injected for you. The only thing you need to do is specify what fields you want the table to list.

## Making a Table

If you are on the index page for a resource, the limit and model will be automatically bound for you.

```php
$table = tr_table();
echo $table;
```

## Custom Model

You can set the model manually as well manually for totally custom tables.

```php
$table = tr_tables(new \App\Model\Posts);
echo $table;
```

## Set Limit

You can set the number of the item that should be displayed in a table with the `setLimit()` method. The `setLimit()` method takes one argument:

1. `$limit` - The number of the item to list `25` (default).

```php
$table->setLimit(35);
```

## Set Order

You can set the order the items should be displayed in a table with the `setOrder()` method. The `setOrder()` method takes two arguments:

1. `$column` - The table column name.
2. `$direction` - The list order `ASC` (default) or `DESC`.

```php
$table->setOrder('id', 'DESC');
```

## Search

You can also limit the fields that can be searched. The `setSearchColumns()` method handles this and takes one argument:

1. `$columns` - An `array` of the columns with custom labels to be searchable.

```php
$table->setSearchColumns([
    'username' => 'Username',
    'email' => 'Email Address',
    'full_name' => 'Full Name',
    'id' => 'ID'
]);
```

Or, you can remove the search.

```php
$table->removeSearch();
```

## Customize Search

You can also add your own search filters for a select menu using the method `appendSearchFormFilter()` or the WordPress action `tr_table_search` hook.

```php
$table->addSearchFormFilter(function() {
echo <<<'EOT'
<select name="my_filter">  
	<optgroup label="Filter By">  
		<option value="some_value">Some Value</option>
	</optgroup>
</select>
EOT;
});
```

Then, you will need to update the model query.

```php
$table->addSearchModelFilter(function($args, $model, $table) {
    /** @var \TypeRocket\Models\Model $model */
	$model->where('field_name', '=', $_GET['my_filter'] ?? null);

	return $args;
});
```

### Only Use Custom Search

Remove the default search filters and only use custom search.

```php
$table->setCustomSearchOnly();
```

## Set Table Columns

By default, every column will be listed from a model. To limit the list of columns displayed, use the `setColumns()` method. The `setColumns()` takes two arguments.

1. `$columns` - An `array` of columns and their settings.
2. `$primary` (optional) - The primary column name.

The primary column should also be the fist listed column under the `$columns` argument. 

```php
$table->setColumns([
    'username' => [
        'label' => 'Username',
        'sort' => true,
        'actions' => ['edit', 'delete', 'view'],
        'delete_ajax' => false, // disable AJAX delete
        'callback' => function( $text, $result ) {
            return $result ? '<strong>'.$text.'</strong>' : '';
        },
        'view_url' => function($show_url, $result) {
            $id = $result->id;
            return "/members/{$id}/";
        }
    ],
    'id' => [
        'label' => 'ID',
        'sort' => true
    ],
], 'username');
```

### Column Settings

Each column can have a set of settings.

 1. **Label** - The `label` setting sets the table's header for that column.
 2. **Sort** - The `sort` setting, if set to `true` will make the column sortable.
 3. **Actions** - The `actions` setting takes an array and can contain the values `view`, `edit`, and `delete`. The `view` setting links to the show resource page.
 4. **Disable AJAX Delete** - There is also a setting to disable delete actions AJAX call and send the user to the delete page instead. The `delete_ajax` setting when set to `false` will disable the AJAX call.
 5. **View URL** -  You can change the view URL with the `view_url` setting. This setting takes a callback with two arguments:  `$show_url` (the URL used for the view link),  `$result`  (the Model of the item for the table row).
 6. **Callback** - You can change the text returned for a column with the `callback` setting. This setting takes a callback with two arguments: `$text`  (the text used for the column), `$result`  (the Model of the item for the table row).

## Set Primary Column

```php
$column = 'id';
$table->setPrimaryColumn($column);
```

## Bulk Actions

This will also add checkboxes to the table.

```php
$actions = [
	'Key' => 'value'
];

$form = tr_form();

$table->setBulkActions($form, $actions);
```