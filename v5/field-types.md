Title: Field Types
Description: TypeRocket has 27+ field types to work with. From repeaters to galleries.

---

## About Field Types

There are 29+ different `Fields` that the `Form` object can create. Each field takes four arguments:

1. **Name** - `string` (required) - This will be the name of the field.
2. **Attributes** - `array` - This will be the HTML attributes applied to the field.
3. **Settings** - `array` - This will be custom settings that can vary between fields.
4. **Label** - `boolean` - This is not for the label text. Set `false` to remove the displaying of the label for the field entirely.

## Field Types

Assuming `Form` is set to a variable called `$form`, here is how to use the fields.

## Text

```php
$form->text('First Name');
```

## Input

The input field can be any HTML5 input type. For example `number`, `datetime-local`, `email`, `date`, `time`, `tel`, `url` and others.

```php
$form->input('Number')->setType('number');

// or...

$form->input('Number')->setTypeNumber();
```

Keep in mind that you can also [set attributes on an input field](https://typerocket.com/docs/v5/fields/#section-instancing-a-field-parameters) by passing an array as the attributes parameter. 

## Password

The password field will never return its value.

```php
$form->password('Your Password');
```

## Hidden

This field should always go at the top or bottom of a form to prevent formatting issues.

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

## Text Expand - Pro Only

This field is like the Text field but it will expand it fit the content contained within it.

```php
$form->textexpand('Expand');
```

## Editor - Pro Only

The editor field uses Redactor 3.

```php
$form->editor('Page Content');
```

### Config & Plugins

You can configure redactor plugins and language under `config/editor.php`.

### Settings 

If you need to set up your own settings for the field, use `setEditorSettings()`.

```php
$settings = (object) ['imageResizable' => false];
$form->editor('Page Content')->setEditorSettings($settings);
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
$radio->setDefault('m');
```

### Radio Images

You can also make radio buttons images instead of the default browser buttons. To do this, use the `useImages()` method and set your options as an array with `src` and `value` indexes.

```php
$form->radio('Photo')->setOptions([
    'One' => [
        'src' => tr_assets_url('components/builder/content.png'),
        'value' => 1
    ],
    'Two' => [
        'src' => tr_assets_url('components/builder/content.png'),
        'value' => 2
    ]
])->useImages()->setDefault(2);
```

## Checkbox

```php
$form->checkbox('Email Me')->setText('Yes');
```

## Checkboxes - Pro Only

Add multiple checkboxes.

```php
$form->checkboxes('Settings')->setOptions([
  'email_notifications' => 'Send me notifications by email.',
  'email_marketing'     => 'Send me marketing emails.',
]);
```

or, with advanced settings:

```php
$form->checkboxes('Settings')->setOptions([
  'email_notifications' => [
    'text' => 'Send me notifications by email.',
    'default' => true
  ]
  'email_marketing'=> [
    'text' => 'Send me marketing emails.',
    'default' => false
  ]
]);
```

## Toggle

```php
$form->toggle('Toggle')->setText('Yes');
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

### Multiple

You can also set the `select` field to multiple values using the `multiple` method.

```php
$form->select('Gender')->multiple();
```

### Searchable

To have a select menu, use `chosen.js`.

```php
$form->select('Gender')->searchable();
```

### Default Value

Select dropdowns can have a default value. To set the default to "Male" specify the value `'m'`.

```php
$select->setDefault('m');
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

Use the `wpEditor()` at your own risk. This editor was never designed to work in a metabox, repeater, or matrix. Also, you should only have one WordPress editor per page.

```php
$wpEditor = $form->wpEditor('Page Content');
```

### Options

You can also set the [WordPress editor internal settings](https://codex.wordpress.org/Function_Reference/wp_editor#Arguments). For example, turning off the media buttons.

```php
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

## Swatches - Pro Only

```php
$form->swatches('Site Color')->setOptions([  
    'Vibrant' => ['#333', '#0073aa'],  
    'Colorful' => ['#523f6d', '#a3b745'],  
]);
```

## Range - Pro Only

```php
$form->range('Years')->setMax(100)->setMin(0)->setStep(1);
```

## Date

```php
$form->date('Release Date');
```

You can set the date pickets format using `setFromat()` and passing any valid [jQuery date format](https://jqueryui.com/datepicker/#date-formats) setting.

```php
$form->date('Release Date')->setFormat('mm/dd/yy');
```

See [full list of options on the jQuery documentation](https://api.jqueryui.com/datepicker/#utility-formatDate) page.

## Image

Images are saved by their attachment ID.

```php
$image = $form->image('Photo');
```

### Button Text

```php
$image->setSetting('button', 'Insert Image');
```

### Image Side - Admin

To set the image size to be used in the admin. This applies to the `gallery()` and `background()` fields as well.

```php
$image->setAdminImageSize('thumbnail');
```

### Dark Background

Set the image background to dark instead of white. This applies to the `gallery()` field as well.

```php
$image->setBackgroundDark();
```

## Background - Pro Only

This field is like the image field but also includes X and Y coordinates.

```php
$form->background('Background');
```

## File

Files are saved by their attachment ID.

```php
$file = $form->file('PDF');
```

### Restricted Mime Types

When adding a file field, you can set the allowed mime types to be uploaded:

```php
echo $form->file('File')->setSetting('type', 'text/csv');
echo $form->file('File')->setSetting('type', 'image/svg+xml');
```

Any of [these mime types](https://codex.wordpress.org/Function_Reference/get_allowed_mime_types) will work.

### Button Text

```php
$file->setSetting('button', 'Insert File')
```

## File Upload

Upload files using the `<input type="file">` field. Unlike other fields this one does not save data to the database nor does it save any field value. You will need to implement your own file upload system.

```php
$file = $form->fileUpload('PDF');
```

When the field is attached to a form it will make that form `multipart/form-data` if you create your own form element using `$form->open()` or the like. Otherwise, you will need to allow file uploads on your form manually.

You can add `multipart/form-data` support to your form directly using:

```php
$form->allowFileUploads()
echo $form->open();
echo $form->close();
```

In the case of the post edit screen you will be using the WordPress provided form element. The same is true in many other places with the WordPress admin. Each form will require `enctype="multipart/form-data"` to be added using hooks.

```php
function add_form_multipart() {
 echo ' enctype="multipart/form-data"';
}

add_action('post_edit_form_tag', 'add_form_multipart');
add_action( "{$taxonomy}_term_edit_form_tag", 'add_form_multipart');
add_action( "{$taxonomy}_term_new_form_tag", 'add_form_multipart');
add_action( "user_edit_form_tag", 'add_form_multipart');
add_action( "user_new_form_tag", 'add_form_multipart');
```

*Note: Some places in WordPress do not allow for file uploads. Comments for example.*

### Multiple

Allow multiple files for uploading.

```php
$form->fileUpload('Image')->multiple();
```

### Accept

You can define what types of files are accepted by the input field.

```php
$form->fileUpload('Image')->accepts(['.png','.jpg','.jpeg','.gif']);
```

### Do Before

To echo information before the input field provide a callback that returns a string. The callback has two parameters:

1. `$field` - An instance of the `FileUpload` field.
2. `$value` - A field value that shares the fields name. **Note**: No value is saved by default, you will need to implement your own value saving system. `$value` is provided for easy value access, provided you save your own data following the TypeRocket model system conventions.

```php
$form->fileUpload('PDF')->doBefore(function($field, $value) {
    return esc_html($value);
});
```

## Gallery - Pro Only

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

### Limit Items

You can also limit the number allowed using the `setLimit()` method.

```php
$form->items('Top 3 Movies')->setLimit(3);
```

## Search & Relationship Field

the search field is the relationship field of TypeRocket. For a single value field,

```php
$form->search('Search');
```

For a multi value field,

```php
$form->search('Links')->multiple();
```

### Search Post Types

You can search all post types - drafts will be included.

```php
$form->search('Search')->setPostTypeOptions('any');
```

Also, you can search for a specific post type.

```php
$form->search('Search')->setPostTypeOptions('post');
```

### Search Taxonomies

You can search a taxonomy's terms.

```php
$form->search('Search')->setTaxonomyOptions('post_tag');
```

### Search Model

You can search a taxonomy's terms.

```php
$model_class = '\TypeRocket\Models\WPPost';
$form->search('Search')->setModelOptions($model_class);
```

### Search URL Endpoint

You can set a custom URL endpoint for your search too.

```php
$example_url = site_url('my-api/fonts');
$form->search('Font')->setUrlOptions( $example_url );
```

Your custom endpoint should have the following return JSON in format simular to the following:

```json
{  
    "search_type":"post_type",  
    "items": [ { "title":"<b>Hello world!</b> (post)", "id":1, "url": null } ],
    "count": "1 in limit of 10"  
}
```

## URL - Pro Only

Like the search field the URL field can be used to look up the URL of a post or term record. The URL field supports the same features as the search field.

```php
$form->url('URL');
```

*Note: When using the URL field it will only search for a record if the input does not start with `/` or `#`.*

### Search URL Filter

You can filter how the URL is returned by the field search. For example, you can make URLs return a relative path.

```php
add_filter('typerocket_search_field_result_url', function($url, $value, $options, $error) {
    $base = home_url();

    if(\TypeRocket\Utility\Str::starts($base, $url)) {
        return \TypeRocket\Utility\Str::replaceFirst($base, '', $url);
    }

    return $url;
}, 10, 4);
```

## Location - Pro Only

Full address field.

```php
$form->location('Location');
```

To enable google maps add your Google Maps API key to your `wp-config.php` file:

```php
define('TYPEROCKET_GOOGLE_MAPS_API_KEY', 'yourapikeyhere');
```

You can also add your API key in the TypeRocket default theme options or under `config/external.php`.

## Repeaters

The repeater field lets you create groups of repeating fields.

For example, what if you're building an event listing site and need to list speakers for each event. A repeater field would be perfect for listing the speakers in an event post type since every speaker's information will be different even if they speak at multiple events.

The repeater could be a group of fields for a conference speaker's name, photo, and a link to their slides. Take a look at adding a meta box with the repeater.

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

### Titles

You can set a headline for each repeater item as well:

```php
$repeater->setTitle('Speaker');
```

### Limit Items

You can also limit the number allowed using the `setLimit()` method.

```php
$repeater->setLimit(10);
```

### Tabs

You can add tabs to repeaters as well using the Tabs API. 

```php
$form = tr_form();

// Basic
echo $form->repeater('Speakers')->setFields([
    $form->image('Photo'),
    $form->text('Name'),
    $form->text('Slides URL')
]);

// With Layout Tabs
$content = [
    $form->textarea('Quote', ['maxlength' => 200]),
    $form->row(
        $form->text('First Name'),
        $form->text('Last Name')
    )
];

$image = [
    $form->image('Avatar'),
    $form->gallery('Gallery'),
];

$tabs = tr_tabs();
$tabs->tab('Content', 'pencil', $content);  
$tabs->tab('Images', 'droplet', $image);

echo $form->repeater('Stories')
    ->setFields($tabs)
    ->setTitle('Story');
```

## Matrix and Builder

Matrix and builder fields work a lot like repeater fields. However, they are not limited to the same field groups like repeaters are.

The matrix field lets you create modular sections. The Builder lets you create modular designs in an easy to use way. The main difference between the two fields is their front end design. A builder uses a slide deck style for creating field groups while the matrix field uses a simple select dropdown. 

```php
$form->matrix('Matrix List');
$form->builder('Page Builder');
```

To add components to these fields see the [Page Builder plugin documentation](/docs/v5/extension-page-builder/#section-components).

## Repater, Builder, Matrix JavaScript Hook

You can also use the [javascript hooks](/docs/v5/javascript-hooks/) to do something when a repeater, matrix, or builder field group is added.