Title: Forms
Description: Create your posts, users, comments and options resources with Forms.

---

## About Forms

With TypeRocket “Forms” you can update or create posts, users, comments, options and your own custom resources.

“Forms” are used in conjunction with “Fields” to interact with TypeRocket “Controllers” and “Models”.

*Note: In TypeRocket 3 resources are singular vs plural.*

There are 6 controllers included with TypeRocket: `PostTypesController`, `PostsController`, `PagesController`, `UsersController`, `CommentsController` and `OptionsController`. These controllers use the WordPress API to create and update resources. By using “Forms” to interact with controllers you are able to perform two of these actions: update and create.

There are also six models included with TypeRocket: `PostTypesModel`, `PostsModel`, `PagesModel`, `UsersModel`, `CommentsModel` and `OptionsModel`. These models correspond to controllers and are used by the `Form` to populate `Fields` with their values. 

## Creating a Form

To create a form you need to instance a new `Form` object. `tr_form()` can be used to do just that.

```php
tr_form()
```

## Auto Resource

*Note: In TypeRocket 3 resources are singular vs plural.*

Defining a resource tells the `Form` what `Controller` and `Model` you plan to use.

When you instance a new form it automatically tries to find the correct resource for your context. It detects if you are working from within a post, user's profile, comment or custom page.

By default:

- Post context uses `PostsController` and `PostsModel`
- Page context uses `PagesController` and `PagesModel`
- Custom Post Type context uses `PostsController` and `PostsModel`
- User context uses `UsersController` and `UsersModel`
- Comment context uses `CommentsController` and `CommentsModel`
- Custom page context uses `OptionsController` and `OptionsModel`

## Specify a Resource

In some cases, you will not want to automatically set controllers. In these cases, you can specify what controller, action, and item you want the `From` to interact with.

Take a look at instancing a `From` that is configured to update a post with the ID of 4.

*Note: In TypeRocket 3 resources are singular vs plural. For example, posts would be post.*

```php
/**
* Instance the From
*
* @param string $resource posts, users, comments or options
* @param string $action update or create
* @param null|int $item_id you can set this to null or an integer
*
* @return From $this
*/
tr_form('posts', 'update', 4);
```

Or, you can configure it to work with options. Options do not need an ID specified since the `wp_options` table and it is a key value store only. Also, for options using update and create do the same action; so, you don’t need to set the action.

```php
tr_form('options')
```

When creating a resource like a user, you don’t need to set an ID because there is no resource yet.

```php
tr_users('users', 'create');
```

Another example: Creating a `From` to update a comment of ID 24 is simple.

```php
tr_form('comments', 'update', 24);
```

## Working inside existing HTML forms

When you instance a `Form` you do not create an HTML `<form>` element immediately; nor do you want to.

For example, you might be creating `Fields` only. Take a scenario where this is the case; when adding fields to a custom post type through a `Metabox` or `setTitleFrom()`.

In the case of a custom post type an HTML `<form>` is already created for you on the post edit page. All you want to do is add `Fields`. You do not need to create a `<form>`; nor should you. You cannot have a `<form>` inside a `<form>`.

Take a look at an example `From` inside a custom post type, "book", where this is the case.

```php
tr_post_type('Book')->setTitleForm(function(){
    $form = tr_form();
    echo $form->text('Author');
});
```

Here you created a custom post type called “book” and added a text field called “Author”.

Because you are in the context of “post” the `Form` object automatically sets the controller to “posts” and configures the action and ID for you too.

Since you are in the post edit page, WordPress already created a `<form>` element and you do not need to create one.

## Opening and closing your own form element

You will not always be working inside another `<form>`. In the case of a custom admin page you will want to echo an opening `<form>` element.

Use the `open()` method of the `Form` object to get the opening tag and `close()` to get the closing tag.

```php
$form = tr_form('posts', 'create');
echo $form->open();
echo $form->close();
```

This will output the configured `<form>` element.

```html
<form
    action="/"
    method="POST"
    data-method="POST"
    class="typerocket-rest-form"
    data-api="/typerocket_rest_api/v1/posts/"
>
<input type="hidden" name="_method" value="POST" />
<input type="hidden"
       id="_tr_nonce_form"
       name="_tr_nonce_form"
       value="dcddd95593"
/>
</form>
```

### Adding a submit button

To quickly add a submit button you can specify a string as a parameter in `close()`

```php
$form = tr_form('posts', 'create');
echo $form->open();
echo $form->close('Submit');
```

When you submit a form request using TypeRocket, it will send the request the the TypeRocket REST API using AJAX.

### Changing the open element defaults

The `open()` method for the `Form` object takes two arguments. 

1. `array` - You can change the form element attributes by passing an `array`. 
2. `boolean` - You can disable AJAX and the REST API by setting the argument to `false`.

Take a look at adding a new attribute and disabling the REST API.

```php
$form = tr_form('posts', 'create');
echo $form->open(array('id' => 'custom'), false);
echo $form->close('Submit');
```

```html
<form action="/" method="POST" id="custom" data-method="POST">
<input type="hidden" name="_method" value="POST">
<input type="hidden"
       id="_tr_nonce_form"
       name="_tr_nonce_form"
       value="e9eeb1e255">
<input type="submit"
       name="_tr_submit_form"
       value="Submit"
       id="_tr_submit_form"
       class="button button-primary">
</form>
```

## Forms and Fields

There are 19 different `Fields` that the `Form` object can create:

1. `text()`
2. `password()`
3. `hidden()`
4. `submit()`
5. `textarea()`
6. `editor()`
7. `radio()`
8. `checkbox()`
9. `select()`
10. `wpEditor()`
11. `color()`
12. `date()`
13. `image()`
14. `file()`
15. `gallery()`
16. `items()`
17. `matrix()`
18. `repeater()`
19. `field()`

Each field takes four arguments, except for `field()`:

1. Name - `string` (required) - This will be the name of the field.
2. Attributes - `array` - This will be the HTML attributes applied to the field.
3. Settings - `array` - This will be custom settings that can very between fields.
4. Label - `boolean` - This is not the label text. Set `false` to disable the label.

The `field()` method takes one argument:

1. Field - `Field` - This is any `Field` object including custom ones that you create.

Every time you call one of the field methods from the `From` object a `Field` object is returned.

- All return a `\TypeRocket\Fields\Field` object of their Class.

### The Fields

Assuming `Form` is set to a variable called `$form`, here is how to use the fields at the most basic level.

### Text

```php
$form->text('First Name');
```

### Password

The password field will never return its value.

```php
$form->password('Your Password');
```

### Hidden

This field should always go at the top or bottom of a form.

```php
$form->hidden('Hidden Field');
```

### Submit

```php
$form->submit('Submit');
```

### Textarea

```php
$form->textarea('About Me');
```

### Editor

```php
$form->editor('Page Content');
```

### Radio

```php
$options = array(
    'Male' => 'm',
    'Female' => 'f',
    'NA' => '0'
);

$form->radio('Gender')->setOptions($options);
```

### Checkbox

```php
$form->checkbox('Email Me')->setSetting('text' => 'Yes');
```

### Select

```php
$options = array(
    'Male' => 'm',
    'Female' => 'f',
    'NA' => '0'
);

$form->select('Gender')->setOptions($options);
```

### Editor

Use the `wpEditor()` at your own risk. This editor was never designed to work in a metabox, repeater or matrix.

```php
$form->wpEditor('Page Content');
```

### Color

```php
$form->color('Color');
```

### Date

```php
$form->date('Release Date');
```


### Image

Images are saved by their attachment ID.


```php
$form->image('Photo');
```

### File

Files are saved by their attachment ID.

```php
$form->file('PDF');
```

### Gallery

Galleries are groups of images saved by their attachment IDs.

```php
$form->gallery('Gallery');
```

### Items

```php
$form->items('List Movies');
```

### Matrix

The [Matrix field](https://typerocket.com/docs/matrix-field/) lets you create modular designs.

```php
$form->matrix('Page Builder');
```

### Repeater

You can create repeatable groups of fields with [repeaters](https://typerocket.com/docs/repeater-field/).

```php
$fields = array(
  array('text', array('First Name') ),
  array('text', array('Last Name') ),
  array('image', array('Photo') )
);

$form->repeater('People')->setFields($fields);
```

### Field

```php
$field = new TypeRocketFieldsText('Name', $form);

$form->field($field);
```

## Debug Status

You can control whether the `From` shows code hint but using the method `setDebugStatus()`. You can set the value to `false` to disable code hints for the form.

```php
$form->setDebugStatus(false);
```

You can also get the debug status.

```php
$status = $form->getDebugStatus();
```

## Populating Fields

By default, forms will populate fields with the data that is saved to their corresponding resource and ID. You can keep fields from being populated by using the method `setPopulate()`.

```php
$form->setPopulate(false);
```

You can also get the population settings.

```php
$populate = $form->getPopulate();
```

## Rendering

TypeRocket forms have some standard HTML formatting. Take a look at the output of a basic text field: `$form->text('Name')`.

```html
<div class="control-section typerocket-fields-text">
    <div class="control-label">
        <span class="label">Name</span>
    <div>
    <div class="control">
        <input type="text" name="tr[name]" value="">
    </div>
</div>
```

### Set render mode

The `Form` object has a setting called render that can be set to `raw`. When the setting render is set to `raw` the fields instanced by the `Form` will not include any extra HTML.

```php
$form->setRenderSetting('raw');
echo $form->text('Name');
```

```html
<input type="text" name="tr[name]" value="">
```

This gives you total control of the design when you need it.

### Get render mode

You can also get the render mode by using the method `getRenderSetting()`.

```php
$renderMode = $form->getRenderSetting();
```

## Settings

There are five methods for dealing with `Form` settings.

These methods are: `getSettings()`, `setSettings()`, `getSetting()`, `setSetting()` and `removeSetting()`.

1. `getSettings()` returns the full array of settings.
2. `setSettings()` takes an array of settings.
3. `getSetting()` return an setting by its key.
4. `setSetting()` sets an setting by a key.
5. `removeSetting()` removes an setting by its key.

Take a look at using all the methods.

```php
$args = $form->getSettings();
$args = array_merge( $args, array( 'render' => 'raw' ) );
$form->setSettings( $args );
$render = $form->getSetting( 'render' );
$form->removeSetting( 'render' );
$form->setSetting( 'render', $render );
```

## Groups

There are times when you want to have fields grouped together when being saved. A great case for this is when you are saving data to `wp_options` using the `OptionsController`.

Take a look at this `Form`.

```php
$form = tr_form('options');
echo $form->open();
echo $form->text('Address');
echo $form->text('Company Name');
echo $form->image('Company Logo');
echo $form->textarea('About the Company');
echo $form->close('Submit');
```

If you were to submit this form it would save each field to a separate row in the `wp_options` table. This might be exactly what you want. It most cases it is not.

This is were grouping comes into play. You can group fields together with the `Form` method `setGroup()`. This method will group all the fields together so that only one row is created in `wp_options`. When it is saved the `key` will be the group name.

Take a look at grouping.

```php
$form = tr_form('options');

// setting the group
$form->setGroup('[company_info]');

echo $form->open();
echo $form->text('Address');
echo $form->text('Company Name');
echo $form->image('Company Logo');
echo $form->textarea('About the Company');
echo $form->close('Submit');
```

### Bracket Syntax

Groups should be set to a `string` using the TypeRocket bracket syntax. The TypeRocket bracket syntax suggests that you use only lowercase case letters and underscores.

```
[lowercase_underscore_only]
```

### Getting the group

You can also get the current group being used with the method `getGroup()`.

```php
$group = $form->getGroup();
```

## Subgroups

You can also do the inverse of grouping with subgroups. However, there are very few use-cases for doing so.

Typically a subgroup is set with blank brackets. Use the `setSub()` method to set the subgroup. 

```php
$form->setSub('[]');
```

You can also get the subgroup with the `getSub()` `Form` method.

```php
$subgroup = $form->getSub();
```

## Resources

As we talked about in the very beginning “Forms” are used in conjunction with “Fields” to interact with TypeRocket `Controllers` and `Models` through the resource property.

You can set the `Form` resource only when the `Form` is instanced. However, you can get the resource with `getResource` at any time.


```php
$resource = $form->getResource();
``` 

## Actions

Actions are the methods called by the `Controller` when a form is submitted. You set the "Action" only when you instance the `Form`. But, you can also get the action with `getAction` at any time.


```php
$action = $form->getAction();
```

## Item ID

The item ID is the resource identifier. This is typically the post, comment or user ID. You can set the `Form` item ID with `setItemId()` and get the item ID with `getItemId`.

As with property, controllers and actions have the same rules applied to them.

```php
$itemId = $form->getItemId();
```

## Using Resource, Action and Item ID together

Take a look at a quick example of instancing a `Form` with a resource, action and item ID together.

```php
if( current_user_can( 'read' ) ) {
    $form = tr_form('users', 'update', get_current_user_id());
    $form->open();
    $form->text('GitHub Handle');
    $form->close('Save Changes');
}
```