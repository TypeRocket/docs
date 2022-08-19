Title: Custom Resources
Description: Add your own custom resources and database tables to build advanced sites.

---

## Getting Started

! **Pro Only**: This is a Pro only extension feature.
*Note: All your custom resources need to have unique names and cannot share a name with a post type or taxonomy. If you break this rule, you might encounter issues.*

Custom resources are the way you access and manage custom database tables.

![typerocket-resource-finished](https://typerocket.com/wp-content/uploads/2020/02/custom-resources-loop.gif)

To get started, you will be creating a custom resource named "Test". **When making your custom resource, it is crucial it does not share the same name as a post type or taxonomy**.

Now, in your main business logic add the following code:

```php
$parent_page = tr_resource_pages('Test')->setIcon('line-chart');
```

If you like, you can use [TypeRocket migrations](/docs/v5/migrations) to manage your custom tables.

```
php galaxy make:migration add_tests_table
```

Here is the database table schema. Add it to your migration.

```sql
-- Description: Tests Migration 
-- >>> Up >>>  
CREATE TABLE `{!!prefix!!}tests` (  
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,  
  `name` varchar(255) DEFAULT NULL,  
  PRIMARY KEY (`id`)  
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  
-- >>> Down >>>  
DROP TABLE IF EXISTS `{!!prefix!!}tests`;
```

Then run the migration.

```
php galaxy migrate up
```
## Model and Controller

```shell
php galaxy make:model -c base Test
```

This will create the model and controller needed to manage your custom resources.

## Admin Views

In the `resources` folder, create a new folder named `pages` to house the admin views. Next, create a folder in `resources/pages` named `tests`. Finally, create four files: `index.php`, `show.php`, `edit.php` and `add.php`.

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

For the index page you might want to use a [table](/docs/v5/tables/). In the index.php view add the following code:

```php
<?php
$table = tr_table();
$table->setColumns([
    'name' => [
        'sort' => true,
        'label' => 'Name',
        'actions' => ['edit', 'view', 'delete']
    ],
    'id' => [
       'sort' => true,
       'label' => 'ID'
    ]
], 'name');
$table->render();
```

*Note: The delete action will use an ajax call to the controller destroy method by default.*

## Add

Let us make the resource capable of creating new items.

#### Actions

In the `TestController` return a view for the admin for the add page.

```php
public function add() {
    $form = tr_form('test');  
    $button = 'Add';  
  
    return tr_view('tests.form', compact('form', 'button'));
}
```

Next, save the data we pass over from the view using the create method.

```php
public function create(Test $test)
{ 
	$test->name = tr_request()->getFields('name');  
	$test->save();  
	tr_response()->flashNext('Test created!');  
	return tr_redirect()->toPage('test', 'index');
}
```

#### Views

In the `form.php` view add the following code:

```php
<?php
/** @var \App\Elements\Form $form */
echo $form->save($button)->setFields(
    $form->text('Name')
);
```

## Edit

Let us make the resource capable of editing items.

#### Actions

In the `TestController` return a view for the admin for the edit page.

```php
public function edit( Test $test ) {
    $form = tr_form($test);  
	$button = 'Update';
	  
	return tr_view('tests.form', compact('form', 'button'));
}
```

Next, save the data we pass over from the view using the update method.

```php
public function update(Test $test)
{
    $test->name = tr_request()->getFields('name');  
	$test->save();  
	tr_response()->flashNext('Test updated!');  
	return tr_redirect()->toPage('test', 'edit', $test->getID());
}
```

## Show

Let us make the resource capable of being viewed.

#### Actions

```php
public function show(Test $test)
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
public function destroy(Test $test)
{
    $test->delete();
	return tr_response()->flashNext('Test deleted!', 'warning');
}
```

If you plan to use a delete view page as well, add a method called `delete()`.

#### View

You usually will not need the delete view. However, in some cases, it makes sense to have a page for deleting an item. For example, maybe you want a user to confirm their decision by clicking a checkbox before moving forward.

First, create a new method called `delete()`.

```php
public function delete($id)
{
    $form = tr_form('test', 'destroy', $id);
    return tr_view('tests.delete', ['form' => $form] );
}
```

To have a delete view create one last view file called `delete.php`.

```php
echo $form->open();
echo $form->checkbox('Delete Record')->setText("I'm sure I want to delete this record");
echo $form->submit('Delete');
echo $form->close();
```

Next, update your `index` view file and turn off AJAX for the delete method.

```php
<?php
$table = tr_table();
$table->setColumns([
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
], 'name');
$table->render();
``` 

Finally, modify the `destroy()` method.

```php
public function destroy($id)
{
    $delete = tr_request()->getFields('delete');  
  
	if($delete) {  
	    $test->delete();  
	    tr_response()->canRedirect()->flashNext('Test deleted!', 'warning');  
	    return tr_redirect()->toPage('test', 'index');  
	}  
	  
	tr_response()->flashNext('Test not deleted!', 'error');  
	return tr_redirect()->toPage('test', 'delete', $test->getID());
}
```

## TypeRocket REST API

If you want to use the TypeRocket REST with your `Form` classes use the `useRest()` method. For example, you might update your `edit` actions `Form` object to use the TypeRocket REST API:

```php
public function edit(Test $test)  
{  
    $form = tr_form($test)->useRest();
    $button = 'Update';  
  
    return tr_view('tests.form', compact('form', 'button'));  
}
```

For security, custom resources using the REST API are only accessible to admin users. If you wish to change the security settings for custom resources using the REST API [register a custom middleware group](/docs/v5/middleware/#section-register-middleware) using the same name as your custom resource. In the example we have been using, `test` would be the name of the group.

If you encounter issues, the TypeRocket REST API requires strict adherence to the controller action naming scheme: `showRest`, `create`, `update`, and `destroy`. 

Also, be sure your custom resources do not share the name of a post type or taxonomy as the REST API is shared with them. For example, if you create a custom resource named `post` the REST API will update the `post` post type and not your custom resource.

## Override Controller

You can override the controller used by using shorthand.

```php
tr_resource_pages('Test@\App\Controllers\TestController');
```

Or, your class if you are making a plugin.

```php
tr_resource_pages('Test@\MyPlugin\TestController');
```

If you want to access your custom resource using the TypeRocket REST API used by the `Form` classes `useRest()` method, you may need to register your resource if you are not using the standard controller naming convention. To register a custom resource use the `Registry::addCustomResource()` method.

```php
\TypeRocket\Register\Registry::addCustomResource('test', [
    'controller' => '\MyPlugin\TestController', // Controller class
]);
```