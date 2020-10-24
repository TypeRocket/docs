Title: Forms
Description: Create your posts, users, comments and options resources with Forms.

---

## Getting Started

It is **IMPORTANT** to keep in mind. You are creating an HTML form when using TypeRocket to create a form. All of the basic rules of HTML `<form>` creation still apply when using TypeRocket. In many cases, you might not need TypeRocket to generate an open or close `<form></form>` tags because the page already has form tags.

For example, when adding custom form fields to a post type edit page, you do not need to generate open and close tags for the form because those already exist in the document of that specific admin page.

Now, to get started creating custom forms and fields with TypeRocket, you need to create a `Form` instance using `tr_form()` and connect a data resource if you want to populate the form fields with information.

You can create a simple form that does not import information from a data source as follows:

```php
// Bind no data to form 
$form = tr_form([]);

echo $form->open(); // open <form> tag
echo $form->text('Post Title');
echo $form->editor('Post Content');
echo $form->close('Save'); // close </form> tag
```

Inversely, you can import data from a data source like so:

```php
// Bind first blog post data to form
$form = tr_form('post', 1); 

echo $form->open(); // open <form> tag
echo $form->text('Post Title');
echo $form->editor('Post Content');
echo $form->close('Save'); // close </form> tag
```

When TypeRocket generates your opening tag (you should not always echo opening and closing `<form>` tags as mentioned before), it takes care of WordPress nonce creation for you. So, the output of the above example all the HTML you need to get things working. 

Here is an abbreviated version of what you can expect if no data was passed into the `Form` instance:

```php
<form action="/my-url">  
 <input type="hidden" name="_method">  
 <input type="hidden" name="_tr_nonce_form" value="bca143cc000">  
 <input type="text" name="tr[post_title]">  
 <textarea type="text" name="tr[post_content]"></textarea>  
</form>
```

### Working inside existing HTML forms

In the case of a custom post type, the HTML `<form>` is already created for you on the post edit page. All you want to do is add fields. You do not need to create a `<form>`; nor should you. You cannot have a `<form>` inside a `<form>`.

Take a scenario where you want to add fields to a custom post type through `setTitleFrom()` method. Take a look at an example `From` inside a custom post type, "book":

```php
tr_post_type('Book')->setTitleForm(function(){
    $form = tr_form();
    echo $form->text('Author');
});
```

As you can see,  when you are on the post edit page, you do need to use the methods `open()` or `close()`.

### In summery

Now that you know the basics of making custom forms and fields with TypeRocket, we can begin to break down in greater detail how each part works. Then you can begin to make your powerful custom forms anywhere in WordPress.

A great place to start is understanding the `tr_form()` function. Let us take a look at that now.

## Creating a Form

To create a form or custom field, you need to instantiate a new `Form` object. `tr_form()` is used to create a `Form` instance.

```php
// Form instance
$form = tr_form($resource, $action, $id, $model_override);

// Field instance 
$textField = $form->text('First Name');
```

This `Form` object powers the creation of custom fields and the injection of field data from any resource: posts, comments, users, external APIs, you name it.

The `tr_form()` method takes up to 4 arguments:

1. **Data Resource** - `Model`,  `shorthand`, `auto`, `DataCollection`,  or `array`. The source of data to populate custom fields.
2. **Action** - `string` values of: `update`, `delete`, or `create`. The type of request to make when submitting the `<form>`. For example, `update` send a PUT request.
3. **Record ID** - `int` value for the records ID. The record to look up in the database by ID. It only applies when using a `Model` for the data resource.
4. **Model Override** (optional) - This value overrides the `Model` used by a `shorthand` or `auto` resource.

## Data Resource

Defining a data resource tells a `Form` how to bind data to the `Form` so that your custom fields display the correct information.

For example, do you want field data from a post, comment,  user, or something else? The `Form` "data resource" controls the flow of information to fields.

*To get any data into your custom fields a `Form` must have a data resource.*

### Types of Resources

There are a few ways to bind a data resource to a `Form` instance:

1. **Model** - An instance of a `Model` or the `Models` class name as a string.
2. **Shorthand** - A short string value, like `post`, to locate the correct `Model` class to be used.
3. **Auto Resource** - Automatically have TypeRocket bind the correct data resource based on the context of WordPress.
4. **DataCollection** - An instance of a `DataCollection`.
5. **Array** - When passing an array, the array converts into a `DataCollection`.

### Model Resource

A `Model` is a class instance used to get data from the database. So, we can use a `Model` instance to get data from the database and bind that model to our `Form`.

```php
$model = (new \App\Models\Post)->find(1);
$form = tr_form($model);
```

You can also use the class name of a model along with its record ID to do the same as the above:

```php
$form = tr_form('\App\Models\Post', 1);
```

### Shorthand Resource

A shorthand resource is a string value that matches the name of any model in your `app/Models` directory. Shorthands are an abbreviation for the model's class name.

With a shorthand resource, the corresponding `Model` loads. For example, a string of `post` in the following example loads the `\App\Models\Post` model.

```php
// Shorthand resource - `post`
tr_form('post');
```

However, the above does not load any data because no record ID is defined. Like when passing the full model class name, you should also pass along the record ID:

```php
$form = tr_form('post', 1);
```

- The shorthand `option` maps to `\App\Models\Option`:
- The shorthand `user` maps to `\App\Models\User`:
- The shorthand `comment` maps to `\App\Models\Comment`:
- The shorthand `tag` maps to `\App\Models\Tag` 
- The shorthand `category` maps to `\App\Models\Category`.

When adding custom post types, taxonomies, or another resource, remember a shorthand resource is a string value that matches the name of any model in your `app/Models` directory.

### Auto Resource

Auto resourcing allows you to add custom fields without needing to know how to bind a `Model` to a `Form` instance. In short, auto resourcing is the magic that makes binding a form and custom fields to a `Model` automatic within specific contexts. 

So with the following code, you can rest easy that your `Form` just works in the WordPress admin:

```php
$form = tr_form();
```

When you instance a new `Form` it automatically tries to find the correct data resource for your context (context is discovered by looking for specific WP global variables). It detects if your `Form` is within a post, user's profile, comment, term, or custom page.

For example, if you add create `Form` instance using the `tr_form()` method in the context of a post type in the admin, the `Form` object automatically binds that post types data resource's `Model`. 

You to write the following code and remove the need to define a resource in the code because of context detection:

```php
tr_post_type('Person')->setTitleForm(function() {
    // Automatically binds the correct model
    $form = tr_form();
});
```

Without auto resourcing you would need to write this:

```php
tr_post_type('Person')->setTitleForm(function() {
    global $post;
    $model = (new \TypeRocket\Models\WPPost($post->post_type))->find($post->ID);
    $form = tr_form($model, 'update');
});
```

Here are the default auto resource mappings:

- Post context uses `Post` Model.
- Page context uses `Page` Model.
- User context uses `User` Model.
- Comment context uses `Comment` Model.
- Category context uses `Category` Model.
- Tag context uses `Tag` Model.
- Custom page context uses `Option` Model (this is also the defaulted model when no context is found).

For custom resources, custom post types, and custom taxonomies, you need to create new models. However, if a custom post type does not have a custom model, it uses the TypeRocket `WPPost` model. Taxonomies work in the same way, with the default being the `WPTerm` model.

### Data Collection & Arrays

If you are working with a third-party API or another external data source that is not apart of the database, you can use the `DataCollection` class to bind data to a `Form`. 

```php
$data = new \TypeRocket\Utility\DataCollection([  
    'first_name' => 'John Smith'  
]);

$form = tr_form( $data );
echo $form->text('First Name');
```

You can pass an array to a `Form` as the resource, and the form converts the array into a `DataCollection` for you.

```php
$data = [  
    'first_name' => 'John Smith'  
];

$form = tr_form( $data );
echo $form->text('First Name');
```

### Setting the Model

If you need to set the data resource after the a `Form` obejct is initalized use the `setModel()` method. You can pass and `array` or `Model` into this method. you can also pass the `string` class name or a model. 

```php
$form->setModel(['first_name' => 'John']);
echo $form->text('First Name'); // value is 'John'
```

## Action

To specify what action you want, the `<form>` to take when it is submitted set the `$action` argument of the `Form` instance. 

By default, TypeRocket tries to reason about the action you want to take based on the data resource that was used (including auto resources).

When you specify an action, you are effectively overriding the action TypeRocket chose automatically.

There are 3 actions you can choose from:

1. `create` or `post` - Sends a `POST` request.
2. `update` or `put` - Send a `PUT` request.
3. `delete` - Sends a `DELETE` request.

Take a look at instancing a `From` that is configured with the `create` action:

```php
// Sends a POST request
$form = tr_form('post', 'create');
```

You could also omit the `create` action because TypeRocket automatically reasons about what you are trying to do.

```php
// Also, sends a POST request with the
// action omited. TypeRocket magic.
$form = tr_form('post');
```

When using the `update` or `delete` options, you need to define the record ID.

## Record ID

A record ID should be used when you want to read, update, or delete a specific record when using the `shorthand` or `Model` class name as a string.  If you bind a `Model` that already retrieved a record to the `Form`  you do not need to provide a record ID.

The record ID retrieves a specific record from the database. If your data resource already has data, then you do not need to retrieve any data.

```php
// Sends a PUT request
$form = tr_form('post', 'update', 1);
```

You can also omit the action when using a record ID as long as the ID is an `int` value. When doing this, the action automatically gets set to `update`. TypeRocket automatically reasons for what you are trying to accomplish.

```php
// Sends a PUT request
$form = tr_form('post', 1);
```

Important to note is the `Option` model. 

The  `Option` model does not need a record ID specified because it uses the **key-value table** `wp_options`. Key-value tables are not used by an ID but are updated by the **key** column.  Also, for key-value tables using `update` and `create` do the same action; so, you don't need to set the action when using the `Option` model.

### Setting the Record ID

You can override the record ID using `setItemId()` but this method will not reload the model. You will need to use `setModel()` after setting the record ID to load a new model.

## Open & Close Forms

When are create TypeRocket forms within a post type, taxonomy, comment, or user profile, there will be an existing `<form>` tag, and you will not need to create one using TypeRocket. However, you will not always be working inside another `<form>`. 

In the case of a custom admin page, you will want to echo the opening and closing `<form></form>` tags.

Use the `open()` method of the `Form` object to get the opening tag and `close()` to get the closing tag.

```php
$form = tr_form('post');
echo $form->open();
echo '<p>My text</p>';
echo $form->close();
```

This will output the configured `<form>` element. 

```html
<form
    action="https://example.com/"   
    method="POST"   
    accept-charset="UTF-8"   
    class="tr-form-container"  
>
    <input type="hidden" name="_method" value="POST">
    <input type="hidden" id="_tr_nonce_form" name="_tr_nonce_form" value="fe9a607216">
    <p>My text</p>
</form>
```

### Adding a submit button

To quickly add a submit button you can specify a string as a parameter in `close()`

```php
$form = tr_form('post', 'create');
echo $form->open();
echo $form->close('Submit');
```

When you submit a form request using TypeRocket, it will send the request to the current URL.

### Changing the open tag defaults

The `open()` method for the `Form` object takes 3 optional arguments. 

1. **Attributes** `array` (optional) - You can change the form element attributes by passing an `array`. 
2. **Request Params** `array` (optional) - You append request params to the request URL.
3. **Method** `string`  (optional) - Override the request method. Options include: `POST`, `PUT`, `DELETE`, and `PATCH`.

Take a look at adding an `id= "custom"` attribute, request params, and method override to a form element.

```php
echo $form->open(
    ['id' => 'custom'], 
    ['action' => 'doStuff'], 
    'PATCH'
);
```

Outputs,

```html
<form 
    action="https://example.com/?action=doStuff" 
    method="POST" 
    accept-charset="UTF-8" 
    class="tr-form-container" 
    id="custom"
>
<input type="hidden" name="_method" value="PATCH">
...
```

## Change Form URL

To change the base URL form action use the method `toUrl()` method before using the `open()` method.

```php
$form->toUrl('https://example.com/my-path/');
echo $form->open(
    ['id' => 'custom'], 
    ['action' => 'doStuff'], 
    'PATCH'
);
```

If you provide only the path, the URL will be relative to the WordPress site URL.

```php
$form->toUrl('/my-path/');
```

You can also get the current URL setting.

```php
$form->toUrl('/my-path/');

$url = $form->getFormUrl();
// return the url https://example.com/my-path/
```

To set the URL to a named route.

```php
// Route with a named capture group of `id`
$form->toRoute('my.route', ['id' => 1]);
```

To set the URL to an admin page.

```php
$form->toAdmin('themes.php', ['page' => 'theme_options']);
```

To set the URL to a custom TypeRocket page.

```php
$form->toPage($resource, $action, $item_id);
```

## Fields

A form's most powerful feature is the fields it can bind model properties too. There are a number of [built-in field types](/docs/v5/field-types/) that come out of the box and you can [create your own](/docs/v5/custom-fields/) if you like.

### Built-In

There are 27+ different fields that the `Form` object can create using the following methods:

1. `text()` - Text field.
2. `input()` - Input field capable of HTML5 features.
3. `time()` - Input field set to the type of time.
4. `number()` - Input field set to the type of number.
5. `background()` - Background field.
6. `swatches()` - Swatches field.
7. `password()` - Password field.
8. `hidden()` - Hidden field.
9. `submit()` - Submit button.
10. `textarea()` - Textarea field.
11. `editor()` - Editor field.
12. `radio()` - Radio field.
13. `checkbox()` - Checkbox field.
14. `checkboxes()` - List of checkboxes.
15. `select()` - Select field.
16. `wpEditor()` - wpEditor field.
17. `color()` - Color field.
18. `date()` - Date field using jQuery UI.
19. `image()` - Image field.
20. `file()` - File field.
21. `gallery()` - Gallery field.
22. `items()` - Items field.
23. `matrix()` - Matrix field.
24. `repeater()` - Respeater field.
25. `builder()` - Builder field.
26. `search()` - Search field with multi select option.
27. `toggle()` - Toggle field.
28. `locaton()` - Location field with Google Maps API integration.

Each field takes 4 arguments:

1. **Name** - `string` (required) - The name of the field.
2. **Attributes** - `array` - The HTML attributes applied to the field.
3. **Settings** - `array` -Custom settings that can vary between fields.
4. **Label** - `boolean` - This is not for the label text. Set `false` to remove the displaying of the label for the field entirely.

You can read more about each field under [field types](/docs/v5/field-types/).

*When you call any of the field methods from a `From` object the corresponding `Field` object is returned (not the `Form` object).*

### Your Own Custom Fields

If you create your field class, you can use the `custom()` method to apply it to the form. The `custom()` method takes 1 argument:

1. **Field** `Field` - This is any `Field` object, including custom ones that you create.

```php
$field = new \TypeRocket\Fields\Text('Name');

echo $form->custom($field);
```

## Form Encapsulation

Instead of echoing each field, you can pass an array of fields into a `Form` using the `setFields()` method and then call the `render()` method to echo the entire form.

```php
$form = tr_form('post', 1);

$form->setFields([
    $form->text('Post Title'),
    $form->editor('Post Content')
]);

$form->render()
```

The `render()` method has 4 optional arguments:

1. **Attributes** `array` (optional) - You can change the form element attributes by passing an `array`. 
2. **Request Params** `array` (optional) - You append request params to the request URL.
3. **Method** `string`  (optional) - Override the request method. Options include: `POST`, `PUT`, `DELETE`, and `PATCH`.
4. **Submit** `string` (optional) - Set the submit button text value.

```php
$form->render(
    ['id' => 'custom', 'class' => 'my-class'], 
    ['action' => 'doStuff'], 
    'PATCH',
    'Submit Changes'
);
```

## Debug Status

You can control whether the `From` shows code hints but using the method `setDebugStatus()`. You can set the value to `false` to disable code hints for the form.

```php
$form->setDebugStatus(false);
```

You can also get the debug status.

```php
$status = $form->getDebugStatus();
```

## Populating Fields

By default, forms populate fields with the data resource information. You can keep fields from being populated with that data by using the method `setPopulate()`.

```php
$form->setPopulate(false);
```

You can also get the population settings.

```php
$populate = $form->getPopulate();
```

## Groups

There are times when you want to have fields grouped when being saved. An exemplary case for this is when you are saving data to `wp_options` using the `OptionsController`.

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

If you were to submit this form, it would save each field to a separate row in the `wp_options` table. Grouping prevents this from happening.

You can group fields with the `Form` method `setGroup()`. This method groups all the fields so that only one record is created in `wp_options`. When the record saves, the `key` is the group name.

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

*Note: Groups can not be nested or used in the matrix, repeater, or builder fields. Also if a field has a group set it overrides the form's group.*

### Getting the group

You can also get the current group used with the method `getGroup()`.

```php
$group = $form->getGroup();
```

## Groups - Dot Syntax

Groups and subgroups should be set to a `string` using the TypeRocket "dot syntax". The TypeRocket dot syntax suggests that you use only `lowercase case letters` and `underscores`. When you use a `dot` this tells the form to add a subgroup.

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

### Append to Group

You can append to a group using dot syntax with the `appendToGroup()` method.

```php
$form->setGroup('company_info');
$form->appendToGroup('about');
// Group is... company_info.about
```

Likewsie you can `prependToGroup()`.

```php
$form->setGroup('company_info');
$form->prependToGroup('about');
// Group is... about.company_info
```

### Extend Group

You can clone a form and append to the group at the same time.

```php
$form->setGroup('company_info');
$new = $form->extend('about');
// $form group... company_info
// $new group... company_info.about
```

Likewise you can `super()` prepends.

```php
$form->setGroup('company_info');
$form->super('about');
// $form group... company_info
// $new group... about.company_info
```


## Text Content

You may want to generate some text content that uses the same markup as fields, so the design remains elegant. To add text content use the `textContent()` method.

```php
echo $form->textContent('Hello world!');
```

## Rows & Columns

You can put fields into a row using the `row()` method.

```php
echo $form->row( 
    $form->text('First Name'), 
    $form->text('Last Name') 
);
```

You can also add columns and titles to rows.

```php
$row = $form->row()
$row->setTitle('A Row')

$column1 = $row->column(
    $form->text('First'),
    $form->text('Last')
);

$column2 = $row->column(
    $form->image('Image')
);

echo $row;
```

You can also add columns fluently using `withColumn()`. The method returns the row object instead of the column object.

```php
echo $form
->row()
->withColumn(
    $form->text('First'),
    $form->text('Last')
)->withColumn(
    $form->image('Image')
);
```

## Sections

In some cases, you want fields grouped into a `<div>`. This can be for styling with CSS or when you want to toggle a group of fields with conditionals. To make a section use the `section()` method.

```php
echo $form->section([
    $form->text('First'),
    $form->text('Last')
])->setTitle('A Section');
```

## Fieldsets

In some cases, you want fields grouped into a `<fieldset>`. This can be for styling with CSS or when you want to toggle a group of fields with conditionals. To make a section use the `fieldset()` method.

```php
echo $form->fieldset('Title', 'A desription.', [
    $form->text('First'),
    $form->text('Last')
]);
```

## Use Old

When using a custom resource, you can load the old data submitted if the user with [redirected with fields](https://typerocket.com/docs/v4/redirects/#section-with-fields) included.

```php
$form->useOld();
```

## Use Errors

The `useErrors()` method provides a way of enabling **inline error messages** for fields. 

This setting will not (and should not) work with post types, taxonomies, and a few other WordPress specific admin pages. For example, WordPress post types do not require particular fields (as a principle), and TypeRocket, therefore, does not have field validation for post types and other places.

If you want inline validation for WordPress post types and others, you should create your admin pages for editing them and disable the WordPress admin pages for those types of content.

Field validation can be used on any page you use a controller. Often, `useErrors()` will be used in conjunction with `useOld()`.

```php
tr_form()->useOld()->useErrors();
```

### How It Works

`useErrors()` looks for the cookie `tr_redirect_errors` and the `tr_redirect_errors` WordPress transient. The `tr_redirect_errors` cookie and transient can only be accessed once (after this is will be deleted).

If the `tr_redirect_errors` cookie is found the form will use the field errors within that cookie to display inline field errors. The best way to set the `tr_redirect_errors` cookie and apply fields to that cookie is to use [HTTP fields](docs/v1/http-fields) or a [validator](/docs/vs/validator) [redirect errors](/docs/v5/validator/#section-get-errors-redirect-on-error).

If you do not want to use cookies for the validation, you can provide an override array.

### Arguments

1. `$key` (string|optional) - The key to access the errors.
2. `$override` (array|optional) - Override array with a key that matches the key argument.

```php
tr_form()->useOld()->useErrors('fields', [ 
    'fields' => [
        'my_field' => 'is required.'
    ]
]);
```

## Use AJAX

Make the form use the TyperRocket AJAX response system.

```php
$form->useAjax();
```

Or, disable the feature (this is the default).

```php
$form->disableAjax();
```

## Use TypeRocket REST API

You can tell forms to use the [TypeRocket REST API](https://typerocket.com/docs/v5/json-api/) using the `useRest()` method. This method enables AJAX mode for the form as well.

```php
$form->useRest();

// Or, override resource and/or record ID

$form->useRest($resource, $item_id);
```

## Require Confirmation

Opens a confirmation alert when the form is submitted. Confirmation is required to submit the same.

```php
$form->useConfirm();
```

## Translation

If you are translating a form's field labels, localization, to another language, you can use the method `setLabelTranslationDomain()` to set the translation domain for all the field labels attached to the form. The default translation domain is `typerocket-profile`.

```php
$form->setLabelTranslationDomain('my-domain');
$form->text('First Name'); // now localized with  
```

## Clone

To clone the form object use `clone()`;

```php
$form = $form->clone();
```

## Macros

If you want to extend the form and add your methods, you can use macros. To add a macro to forms, use the `setMacro()` method and the `tr_from` hook.

```php
add_action('typerocket_form', function($form) {
	$form->setMacro('myFunction', function($your, $args) {
		// do stuff...
		
		return $this;
	});
});
```

```php
$form->myFunction('your', 'arg');
```

Macros do not override existing methods.

## Attributes

Attributes are the HTML attributes set to the form's open tag.

1. `getAttributes()` returns the full array of attributes.
2. `attrReset()` takes an array of attributes and replaces the old.
3. `attrExtend()` takes an array of attributes and merges the old.
4. `getAttribute()` return an attribute by its key.
5. `setAttribute()` sets an attribute by a key.
6. `removeAttribute()` removes an attribute by its key.
7. `attrClass()` appends a string to the class attribute.
8. `attr()` sets and gets values. 

### Basic attribute methods

Take a look at using some of the basic methods.

```php
$args = $field->getAttributes();
$field->attrExtend( [ 'id' => 'names' ] );
$field->removeAttribute( 'id' );
$classes = $field->attr( 'class' );
$field->attr( 'class', $classes );
```

### Appending a string to the class attribute

Here we use the `attrClass()` method to extend the class attribute.

```php
$field->attrClass('date-picker');
```