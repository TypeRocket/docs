Title: Tables
Description: Tables a used for displaying data from models.

---

## About Tables

Tables are mainly used for a resources admin pages index view. When a table is created within the context of a resource admin view all the required settings are detected and injected for you. The only thing you need to do is specify what fields you want the table to list.

## Making a Table

If you are on the index page for a resource the limit and model will be automatically bound for you.

```php
$tables = tr_tables();
$tables->render();
```

## Custom Model

You can set the model manually as well manually for totally custom tables.

```php
$tables = tr_tables(25, new \App\Model\Posts);
$tables->render();
```

## Set Limit

You can set the number of the item that should be displayed in a table with the `setLimit()` method. The `setLimit()` method takes one argument:

1. `$limit` - The number of the item to list `25` (default).

```php
$tables->setLimit(35);
```

## Set Order

You can set the order the items should be displayed in a table with the `setOrder()` method. The `setOrder()` method takes two arguments:

1. `$column` - The table column name.
2. `$direction` - The list order `ASC` (default) or `DESC`.

```php
$tables->setOrder('id', 'DESC');
```

## Search

You can also limit the fields that can be searched. The `setSearchColumns()` method handles this and takes one argument:

1. `$columns` - An `array` of the columns with custom labels to be searchable.

```php
$tables->setSearchColumns([
    'username' => 'Username',
    'email' => 'Email Address',
    'full_name' => 'Full Name',
    'id' => 'ID'
]);
```

## Set Table Columns

By default, every column will be listed from a model. To limit the list of columns displayed use the `setColumns()` method. The `setColumns()` takes two arguments.

1. `$primary` - The primary column name.
2. `$columns` - An `array` of columns and their settings.

The primary column should also be the fist listed column under the `$columns` argument. 

```php
$tables->setColumns('username', [
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
]);
```

## Column Settings

Each column can have a set of settings.

### Label

The `label` setting sets the table's header for that column.

### Sort

The `sort` setting if set to `true` will make the column sortable.

### Actions

The `actions` setting takes an array and can contain the values `view`, `edit`, and `delete`. The `view` setting links to the show resource page.

### Disable AJAX Delete

There is also a setting to disable delete actions AJAX call and send the user to the delete page instead. The `delete_ajax` setting when set to `false` will disable the AJAX call.

### View URL

You can change the view URL with the `view_url` setting. This setting takes a callback with two arguments:

1. `$show_url` - The URL used for the view link.
2. `$result` - The Model of the item for the table row.

```php
['view_url' => function($show_url, $result) {
    $id = $result->id;
    return "/members/{$id}/";
}];
```

### Callback

You can change the text returned for a column with the `callback` setting. This setting takes a callback with two arguments:

1. `$text` - The text used for the column.
2. `$result` - The Model of the item for the table row.

```php
['callback' => function( $text, $result ) {
    return $value ? '<strong>'.$text.'</strong>' : '';
}];
```
 
## Full Example

```php
<?php
$tables = tr_tables();
$tables->setLimit(35);
$tables->setSearchColumns([
    'username' => 'Username',
    'email' => 'Email Address',
    'full_name' => 'Full Name',
    'id' => 'ID'
]);
$tables->setOrder('id', 'DESC');
$tables->setColumns('username', [
    'username' => [
        'label' => 'Username',
        'sort' => true,
        'actions' => ['edit', 'view'],
        'view_url' => function($url, $result) {
            $id = $result->id;
            return "/members/{$id}/";
        }
    ],
    'id' => [
        'label' => 'ID',
        'sort' => true
    ],
    'email' => [
        'label' => 'Email',
        'sort' => true
    ],
    'full_name' => [
        'label' => 'Full Name',
        'sort' => true
    ],
    'update_requested' => [
        'label' => 'Needs Update',
        'callback' => function( $value ) {
            return $value ? '<i class="badge-yellow">Review</i>' : '';
        },
        'sort' => true
    ],
    'level' => [
        'label' => 'Level',
        'sort' => true,
        'callback' => function( $value ) {
            return ucfirst($value);
        },
    ],
    'validate' => [
        'label' => 'Validated',
        'sort' => true,
        'callback' => function( $value ) {
            return $value != '1' ? '<i class="badge-red">Pending</i>' : 'Yes';
        },
    ]
]);
$tables->render();
```

## Checkboxes

When you want to add checkboxes to your table call the `addCheckboxes` method.

```php
$table->addCheckboxes();
```

To remove them,

```php
$table->removeCheckboxes();
```

## Append to Search Form

```php
$table->appendSearchFormFilters(function(){
    // add custom HTML to search table form
});
```

## Hooks

- `tr_table_search_model` - Update the search model on the fly.
- `tr_table_search_form` - Add HTML to the search form filters area.