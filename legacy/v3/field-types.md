Title: Field Types
Description: TypeRocket has 21 field types to work with. From repeaters to galleries.

---

## About Field Types

There are 21 different `Fields` that the `Form` object can create. Each field takes four arguments:

1. Name - `string` (required) - This will be the name of the field.
2. Attributes - `array` - This will be the HTML attributes applied to the field.
3. Settings - `array` - This will be custom settings that can vary between fields.
4. Label - `boolean` - This is not the label text. Set `false` to disable the label.

## Field Types

Assuming `Form` is set to a variable called `$form`, here is how to use the fields.

## Text

```php
$form->text('First Name');
```

## Password

The password field will never return its value.

```php
$form->password('Your Password');
```

## Hidden

This field should always go at the top or bottom of a form to prevent formating issues.

```php
$form->hidden('Hidden Field');
```

## Submit

```php
$form->submit('Submit');
```

## Textarea

```php
$form->textarea('About Me');
```

## Editor

The editor field uses [Redactor 10](https://imperavi.com/assets/pdf/redactor-documentation-10.pdf).

```php
$form->editor('Page Content');
```

### Settings

You can access the default setting of the editor field as well. By default, Redactor has the following set of buttons:

```js
['html','formatting','bold','italic','deleted',
'unorderedlist','orderedlist','outdent','indent',
'image','file','link','alignment','horizontalrule'];

//additionalbuttons
//'underline'
```

If you need to setup your own set of buttons you can do so using this option:

```js
TypeRocket.redactorSettings = {
    buttons: ['formatting','bold','italic']
}
```

## Radio

```php
$options = [
    'Male' => 'm',
    'Female' => 'f',
    'NA' => '0'
];

$radio = $form->radio('Gender')->setOptions($options);
```

### Default Value

Radio buttons can have a default value. To set the default to "Male" specify the value `'m'`.

```php
$radio->setSetting('default', 'm');
```

## Checkbox


```php
$form->checkbox('Email Me')->setText('Yes');
```

### Default Value

Checkboxes can have a default value. Setting the default setting to `true` will make the checkbox `checked`.

```php
$radio->setSetting('default', true);
```

## Select

```php
$options = [
    'Male' => 'm',
    'Female' => 'f',
    'NA' => '0'
];

$select = $form->select('Gender')->setOptions($options);
```

### Multiple (v3.0.16)

You can also set the `select` field to multiple values using the `multiple` method.

```php
$form->select('Gender')->multiple();
```

### Default Value

Select dropdowns can have a default value. To set the default to "Male" specify the value `'m'`.

```php
$select->setSetting('default', 'm');
```

### Option Groups

```php
$options = [
    'Group Label' => [
        'Male' => 'm',
        'Female' => 'f',
        'NA' => '0'
    ]
];

$form->select('Gender')->setOptions($options);
```

## WordPress Editor

Use the `wpEditor()` at your own risk. This editor was never designed to work in a metabox, repeater or matrix. Also, you should only have one WordPress editor per page.

```php
$wpEditor = $form->wpEditor('Page Content');
```

### Options

You can also set the [WordPress editor internal settings](https://codex.wordpress.org/Function_Reference/wp_editor#Arguments). For example, turning off the media buttons.

```
$wpEditor->setSetting('options', ['media_buttons' => false])
```

## Color

```php
$form->color('Color');
```

### Palette

Color fields can have a palette defined for the color picker. Five or six is best.

```php

$form->color('Color')->setPalette(['#FFFFFF', '#000000']);
```

## Date

```php
$form->date('Release Date');
```

## Image

Images are saved by their attachment ID.


```php
$image = $form->image('Photo');
```

### Button Text

```php
$image->setSetting('button', 'Insert Image')
```

## File

Files are saved by their attachment ID.

```php
$file = $form->file('PDF');
```

### Button Text

```php
$file->setSetting('button', 'Insert File')
```

## Gallery

Galleries are groups of images saved by their attachment IDs.

```php
$gallery = $form->gallery('Gallery');
```

### Button Text

```php
$gallery->setSetting('button', 'Insert Images')
```

## Items

```php
$form->items('List Movies');
```

## Search

```php
$form->search('Search');
```

### Search Post Types

You can search all post types - drafts will be included.

```php
$form->search('Search')->setPostType('any');
```

Also, you can search a specific post type.

```php
$form->search('Search')->setPostType('post');
```

### Search Taxonomies

You can search a taxonomies terms.

```php
$form->search('Search')->setTaxonomy('post_tag');
```

## Repeaters

The repeater field lets you create groups of repeating fields.

For example, what if you're building an event listing site and need to list speakers for each event. A repeater field would be perfect for listing the speakers in an event post type since every speaker's information will be different even if they speak at multiple events.

The repeater could be a group of fields for a conference speakers name, photo and a link to their slides. Take a look at adding a meta box with the repeater.

```php 
$box = tr_meta_box('Speakers');
$box->addScreen( 'event' );
$box->setCallback(function() {
    $form = tr_form();
    $repeater = $form->repeater('Speakers')->setFields([
        $form->image('Photo'),
        $form->text('Name'),
        $form->text('Slides URL')
    ]);

    echo $repeater;
});
```

![Repeater field](https://typerocket.com/wp-content/uploads/2015/07/docs-repeater-field-typerocket.png)

### Fields

There are three methods for dealing with fields. These methods are: `getFields()`, `setFields()` and `appendField()`.

1. `getFields()` returns the full `array` of field arrays.
2. `setFields()` takes an `array` of field arrays.
3. `appendField()` append a field `array`.

### Headlines

You can set a headline for each repeater item as well:

```php
$repeater->setHeadline('Speaker');
```

## Matrix and Builder

Matrix and builder fields work a lot like repeater fields. However, they are not limited to the same field groups like repeaters.

The matrix field lets you create modular sections. The Builder lets you create modular designs in an easy to use way. The main difference between the two fields is their front end design. A builder uses a slide deck style for creating field groups while the matrix field uses a simple select dropdown. 

```php
$form->matrix('Matrix List');
$form->builder('Page Builder');
```

As an example, the [Page Builder Plugin](/docs/v3/builder/) uses the builder field.

### Components

These fields use what are called "Components". Components are the field groups dynamically generated when a new item is added to the matrix or builder field list.

### Component Folders

To start added components there are three directories you need to work with. The directories are: `resources/visuals`, `resources/components`, and `wordpress/assets/components`.

The `resources/visuals` directory is for adding the HTML for each components front-end design. The `resources/components` directory is for adding the backend fields for each component. Finally, the `wordpress/assets/components` directory is for the thumbnails of the components.

*Note: Matrix fields do not require thumbnails.*

### Making a Component

To make components create a folder in each of the folder location with the name of the field created. For example, a field named "Page Builder" would have this folder structure.

```php
$form->builder('Page Builder');
```

- `resources/visuals/page_builder/`
- `resources/components/page_builder/`
- `wordpress/assets/components/page_builder/`

To create a component under this field add files with matching names. For example, a component named "Content" has the files:

- `resources/visuals/page_builder/content.php` - Front-end.
- `resources/components/page_builder/content.php` - Backend fields.
- `wordpress/assets/components/page_builder/content.png` - Thumbnail.

Here is an example for a component named "Banner":

- `resources/visuals/page_builder/banner.php` - Front-end.
- `resources/components/page_builder/banner.php` - Backend fields.
- `wordpress/assets/components/page_builder/banner.png` - Thumbnail.

### Using a compnent

Once the files are created you can begin adding code to the front end and backend files. For example, the "Content" component.

For the backend file, `resources/components/page_builder/content.php`. The `$form` variable will be created for you and the `<h1>` tag with be the component's label on the backend.

```php
<h1>Content Component</h1>
<?php
echo $form->text('Headline');
echo $form->editor('Content');
```

For the front-end file, `resources/visuals/page_builder/banner.php`. The `$data` variable will be created for you can contain all the fields information for you.

```html
<div class="builder-content">
    <h2><?php echo esc_html($data['headline']); ?></h2>
    <hr />
    <?php echo wpautop( $data['content'] ); ?>
</div>
```

## Repater, Builder, Matrix JavaScript Hook

You can also use the [javascript hooks](/docs/v3/javascript-hooks/) to do something when a repeater, matrix, or builder field group is added.