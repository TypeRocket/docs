Title: Matrix Field
Description: Built completely custom layout using the most powerful field... Matrix.

---

The matrix field is the most advanced `Field` that comes out of the box with TypeRocket. With it you can built totally custom designs.

## Use Case

The best example and use case for a matrix field is for [building modular designs](http://alistapart.com/article/language-of-modular-design).

Maybe you have just created a single page website with three different sections.

- The first, is for a large banner of text.
- The second, is a list of testimonials.
- The third, is a contact section.

By using a matrix field you can take each of these sections and turn them into individual modular components that you can reuse on any number of pages.

## Page Builder

Take a look at removing the editor for the "page" post type and using matrix instead. This will allow you to start designing the three sections.

In your themes `functions.php` file:

```php
<?php // functions.php

// Adding Matrix field to existing "page" post type 
add_action('edit_form_after_title', function($post) {

    echo '<div class="typerocket-container">';

    if($post->post_type == 'page') {
        $form = tr_form();
        echo $form->matrix('Page Builder');
    }

    echo '</div>';

});

// Remove the WordPress Editor from the "page" post type
add_action('init', function() {
    remove_post_type_support('page', 'editor');
});
```

### Debug

When you go to add or edit a page TypeRocket debug will tell you that you need to create a matrix group called `page_builder`.

*Note: The group name will always be the name of the matrix field.*

![Matrix debug](https://l.rb.typerocket.test/wp-content/uploads/2015/08/docs-matrix-example-debug.png)

Here TypeRocket is telling you that you need to create a folder named `page_builder` in the matrix folder you have specified in the `config.php` file.

### Setup

By default, there is no `matrix` folder so you will need to create it as well.

In the `typerocket` folder where the `config.php` file is located create a folder named `matrix`; then create a folder with the matrix folder named `page_builder`.

![Matrix folder](https://l.rb.typerocket.test/wp-content/uploads/2015/08/docs-matrix-folder.png)

Now that you have created a matrix group folder you can start adding files to it. These files will be the options used by the matrix field.

### Creating matrix options

To build the sections for the single page design you should create three files: `banner.php`, `contact.php` and `testimonials.php`.

![Matrix options](https://l.rb.typerocket.test/wp-content/uploads/2015/08/docs-matrix-options.png)

The Matrix field will pick up on these files and populate the options for you. This is what the front-end will look like.

![Matrix front-end options](https://l.rb.typerocket.test/wp-content/uploads/2015/08/docs-matrix-options-front-end.png)

### Setting up each matrix option

Now for the fun part. In each matrix option file you can add fields using the `$form` variable. The `$form` variable is an instance of the `From` object.

In the `banner.php` file you could add fields for a main headline and description for example.

```php
<?php // banner.php
echo $form->text('Main Headline');
echo $form->text('Description');
```

For the `contact.php` file you might create some fields for an email and phone number.

```php
<?php // contact.php
echo $form->text('Email');
echo $form->text('Phone Number');
```

For the `testimonials.php` file you might add a photo of the person, their name and what they had to say. But you want to add as many people as you like so a repeater field is what you will use.

```php
<?php // testimonials.php
echo $form->repeater('Testimonials')->setFields(array(
    $form->image('Photo'),
    $form->text('Name'),
    $form->text('Quote')
));
```

### Options added

After you have configured the option files when an option is selected and you click "Add New" the matrix option field group will be created. You can also use the [javascript hooks](https://l.rb.typerocket.test/docs/javascript-hooks/) to do something when a group is added.

Take a look at what you get if you add all three options. Now you can add custom content to each section and update your design as you like.

![Matrix add option](https://l.rb.typerocket.test/wp-content/uploads/2015/08/docs-matrix-options-added.png)