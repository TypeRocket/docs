Title: Forms 
Description: Create your posts, users, comments and options resources with Forms.

---

## About Forms

With TypeRocket "Forms" you can update or create posts, users, terms, comments, options and your own custom resources.

"Forms" are used in conjunction with "Fields" to interact with TypeRocket "Controllers" and "Models".

### Models and Controllers

There are seven controllers included with TypeRocket: `PostController`, `PageController`, `UserController`, `CommentController`, `CategoryController`, `TagController` and `OptionController`. These controllers use the WordPress API to create and update resources. By using “Forms” to interact with controllers you are able to perform two of these actions: create, update, and delete.

There are also seven models included with TypeRocket: `Post`, `Page`, `User`, `Comment`, `Category`, `Tag` and `Option`. These models correspond to controllers and are used by the `Form` to populate fields with their values. 

## Creating a Form

To create a form you need to instance a new `Form` object. `tr_form()` can be used to create an instance.

```php
$form = tr_form()
```

## Auto Resource

Defining a resource tells the `Form` what `Controller` and `Model` you plan to use.

When you instance a new `Form` it automatically tries to find the correct resource for your context. It detects if you are working from within a post, user's profile, comment, term or custom page.

By default:

- Post context uses `PostController` and `Post`
- Page context uses `PageController` and `Page`
- User context uses `UserController` and `User`
- Comment context uses `CommentController` and `Comment`
- Category context uses `CategoryController` and `Category`
- Tag context uses `TagController` and `Tag`
- Custom page context uses `OptionController` and `Option`

For custom resources, custom post types, and custom taxonomies you will need to create new controllers and models. However, if a custom post type does not have a defined model and controller it will use `PostController` and `Post`.

## Specify a Resource

You can specify what controller, action, and item you want the `From` to interact with. 


### Post

Take a look at instancing a `From` that is configured to update a post with the ID of 4.

```php
$form = tr_form('post', 'update', 4);
```

### Option

Or, you can configure it to work with options. Options do not need an ID specified because the `wp_options` table is a key value store. Also, for options using update and create do the same action; so, you don’t need to set the action.

```php
$form = tr_form('option')
```

### User

When creating a resource like a user, you don’t need to set an ID because there is no resource yet.

```php
tr_form('user', 'create');
```

### Comment

Another example: Creating a `From` to update a comment of ID 24 is simple.

```php
tr_form('comment', 'update', 24);
```

## Working inside existing HTML forms

When you instance a `Form` you do not create an HTML `<form>` element immediately; nor do you want to.

For example, you might be creating `Fields` only. Take a scenario where this is the case; when adding fields to a custom post type through a `MetaBox` or `setTitleFrom()`.

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

When you are on the post edit page, WordPress already created a `<form>` element and you do not need to create one.

## Opening and closing your own form element

You will not always be working inside another `<form>`. In the case of a custom admin page you will want to echo an opening `<form>` element.

Use the `open()` method of the `Form` object to get the opening tag and `close()` to get the closing tag.

```php
$form = tr_form('post', 'create');
echo $form->open();
echo $form->close();
```

This will output the configured `<form>` element.

```html
<form
    action="/"
    method="POST"
    data-method="POST"
    class="typerocket-ajax-form"
    data-api="/tr_json_api/v1/post/"
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
$form = tr_form('post', 'create');
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
$form = tr_form('post', 'create');
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

## Fields

A form's most powerful feature is the fields it can bind model properties too. There are a number of [built-in field types](/docs/v4/field-types/) that come out of the box and you can [create your own](/docs/v4/custom-fields/) if you like.

### Built-In

There are 23 different `Fields` that the `Form` object can create:

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
20. `builder()`
21. `search()`
21. `toggle()`
22. `links()`
23. `locaton()`

Each field takes four arguments, except for `field()`:

1. Name - `string` (required) - This will be the name of the field.
2. Attributes - `array` - This will be the HTML attributes applied to the field.
3. Settings - `array` - This will be custom settings that can vary between fields.
4. Label - `boolean` - This is not the label text. Set `false` to disable the label.

You can read more about each field under [field types](/docs/v4/field-types/).

### Custom

The `field()` method takes one argument:

1. Field - `Field` - This is any `Field` object including custom ones that you create.

Every time you call one of the field methods from the `From` object a `Field` object is returned. All return a `TypeRocket\Fields\Field` object of their Class.

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
$form = tr_form('option');
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
$form = tr_form('option');

// setting the group
$form->setGroup('company_info');

echo $form->open();
echo $form->text('Address');
echo $form->text('Company Name');
echo $form->image('Company Logo');
echo $form->textarea('About the Company');
echo $form->close('Submit');
```

*Note: Groups can not be nested or used in matrix, repeater or builder fields. Also if a field has a group set it will override the from group.*

### Getting the group

You can also get the current group being used with the method `getGroup()`.

```php
$group = $form->getGroup();
```

## Dot Syntax

Groups and sub groups should be set to a `string` using the TypeRocket "dot syntax". The TypeRocket dot syntax suggests that you use only `lowercase case letters` and `underscores`. When you use a `dot` this tells the form to add a sub group.

```
group_name.another.lowercase_underscore_only
```

### Example 1: No dots

```php
$form->setGroup('company_info');
$form->text('Name')
```

The above translates to,

```html
<input name="tr[company_info][name]" />
```

### Example 2: With dots

```php
$form->setGroup('company_info.about');
$form->text('Name')
```

The above translates to,

```html
<input name="tr[company_info][about][name]" />
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
    $form = tr_form('user', 'update', get_current_user_id());
    $form->open();
    $form->text('Github Handle');
    $form->close('Save Changes');
}
```

## Field Rows

In some cases, you will want fields side by side. For this use the `row()` method.

```php
$form->row( $form->text('First Name'), $form->text('Last Name') );
```

### Row Columns and Titles

You can also add columns and titles to form rows.

```php
$form = tr_form();
    echo $form->row(
        $form->column($form->text('First'), $form->text('Last'))->setTitle('A title'),
        $form->column($form->image('Image'))
    )->setTitle('A Row');
    echo $form->row(
        $form->column($form->text('First'), $form->text('Last')),
        $form->column($form->image('Image'))
    )->setTitle('A Row');
```

## Use Old

When using a custom resource you can load the old data submitted if the user with [redirected with fields](https://l.rb.typerocket.test/docs/v4/redirects/#section-with-fields) included.

```php
$form->useOld();
```

## Use URL

To override how and to what URL as form is submitted call the `useUrl()` method. The `useUrl()` method takes two arguments:

1. `$method` - A string with `post`, `put` or `delete`.
2. `$url` - The URL of the domain to submit the request to.

This method is great when making forms for the front-end of your site.

```php
$form = new Form('post', 'update', 1);
$form->useUrl('put', '/posts/1/edit/'); // override
```

## Use JSON API

You can tell forms to use the [TypeRocket JSON API](https://l.rb.typerocket.test/docs/v4/json-api/) as well with the `useJson()` method. This method will also enable AJAX mode for the form.

```php
$form->useJson();
```

