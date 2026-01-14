Title: Custom Resources
Description: Add your own custom resources and database tables to build advanced sites.

---

## About Resources

Simply put, resources are the way you access and manage custom database tables.

![typerocket-resource-finished](https://typerocket.com/wp-content/uploads/2016/09/typerocket-resource-finished.gif)

## Register a Resource

In your main business logic add the following code:

```php
tr_resource_pages('Test')->setIcon('line-chart');
```

Here is the schema we will be working with:

```sql
CREATE TABLE `wp_tests` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

If you like you can use [TypeRocket migrations](/docs/v4/migrations) to manage your custom tables.

*Note: All your custom resources need to have unique names and can not share a name with a post type or taxonomy. If you break this rule you might encounter issues.*

## Model and Controller

```shell
php galaxy make:model -c base Test
```

This will create the model and controller needed to manage your custom resources.

## Admin Views

In the `resources` folder create a new folder named `pages` to house the admin views. Next, create a folder in `resources/pages` named `tests`. Finally create four files: `index.php`, `show.php`, `edit.php` and `add.php`.

## Index

Let us get started building out a complete resource.

#### Actions

In the `TestController` you just created return a view for the admin for the index page.

```php
public function index() {
    return tr_view('tests.index');
}

```

#### View

For the index page you might want to use a [table](/docs/v4/tables/). In the index.php view add the following code:

```php
<?php
$table = tr_tables();
$table->setColumns('name', [
    'name' => [
        'sort' => true,
        'label' => 'Name',
        'actions' => ['edit', 'view', 'delete']
    ],
    'id' => [
       'sort' => true,
       'label' => 'ID'
    ]
]);
$table->render();
```

*Note: The delete action will use an ajax call to the controller destroy method by default.*

## Add

Let us make the resource capable of creating new items.

#### Actions

In the `TestController` return a view for the admin for the add page.

```php
public function add() {
    $form = tr_form('test', 'create');
    return tr_view('tests.add', ['form' => $form]);
}
```

Next, save the data we pass over from the view using the create method.

```php
public function create()
{
    $test = new \App\Models\Test;
    $test->name = $this->request->getFields('name');
    $test->save();
    $this->response->flashNext('Test created!');
    return tr_redirect()->toPage('test', 'index');
}
```

#### Views

In the `add.php` view add the following code:

```php
<?php
echo $form->open();
echo $form->text('Name');
echo $form->submit('Add');
echo $form->close();
```

## Edit

Let us make the resource capable of editing items.

#### Actions

In the `TestController` return a view for the admin for the edit page.

```php
public function edit( $id ) {
    $form = tr_form('test', 'update', $id);
    return tr_view('tests.edit', ['form' => $form] );
}
```

Next, save the data we pass over from the view using the update method.

```php
public function update($id, \App\Models\Test $test)
{
    $test->name = $this->request->getFields('name');
    $test->save();
    $this->response->flashNext('Test updated!');
    return tr_redirect()->toPage('test', 'edit', $id);
}
```

#### View

In the `edit.php` view add the following code:

```php
<?php
echo $form->open();
echo $form->text('Name');
echo $form->submit('Update');
echo $form->close();
```

## Show

Let us make the resource capable of being viewed.

#### Actions

```php
public function show($id, \App\Models\Test $test)
{
    return tr_view('tests.show', ['test' => $test] );
}
```

#### View

```php
<h3><?php echo esc_html($test->name); ?></h3>
```

## Delete

Let us make a resource capable of being deleted.

#### Actions

```php
public function destroy($id)
{
    $test = new \App\Models\Test();
    $test->findOrDie($id);
    $test->delete();
    $this->response->flashNext('Test deleted!', 'warning');
    return tr_redirect()->toPage('test', 'index');
}
```

If you plan to use a delete view page as well add a method called `delete()`.

#### View

Normally you will not need the delete view. However, in some cases, it makes sense to have a page for deleting an item. For example, maybe you want a user to confirm their decision by clicking a checkbox before moving forward.

First, create a new method called `delete()`.

```php
public function delete($id)
{
    $form = tr_form('test', 'destroy', $id);
    return tr_view('tests.delete', ['form' => $form] );
}
```

If you wish to have a delete view create one last view file called `delete.php`.

```php
echo $form->open();
echo $form->checkbox('Delete Record')->setText("I'm sure I want to delete this record");
echo $form->submit('Delete');
echo $form->close();
```

Next, update your `index` view file and turn off AJAx for the delete method.

```php
<?php
$table = tr_tables();
$table->setColumns('name', [
    'name' => [
        'sort' => true,
        'label' => 'Name',
        'actions' => ['edit', 'view', 'delete'],
        'delete_ajax' => false // disable AJAX
    ],
    'id' => [
       'sort' => true,
       'label' => 'ID'
    ]
]);
$table->render();
``` 

Finally, modify the `destroy()` method.

```php
public function destroy($id)
{
    $test = new \App\Models\Test();    
    $delete = $this->request->getFields('delete_record');

    if( $delete == '1' ) {
        $test->findOrDie($id);
        $test->delete();
        $this->response->flashNext('Test deleted!', 'warning');
        return tr_redirect()->toPage('test', 'index');
    } else {
        $this->response->flashNext('Test not deleted!', 'error');
        return tr_redirect()->toPage('test', 'delete', $id);
    }
}
```

### TypeRocket JSON API

If you want to access your custom resource using the TypeRocket JSON API used by the `Form` classes `useJson()` method you will need to register your resource. To register a custom resource use the `Registry::addCustomResource()` method.

```php
Registry::addCustomResource('test', [
        'test', // singular
        'tests', // plural
        '\App\Models\Test', // Model class
        '\App\Controllers\TestController', // Controller class
]);
```

For example, once registered you might update your `add` actions `Form` object to use the TypeRocket JSON API:

```php
public function add() {
    $form = tr_form('test', 'create')->useJson();
    return tr_view('tests.add', ['form' => $form]);
}
```

If you encounter issues keep in mind the TypeRocket JSON API requires strict adherence to the controller action naming scheme: `showRest`, `create`, `update`, and `destroy`.



 