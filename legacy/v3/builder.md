Title: Page Builder for WordPress
Description: Build modular designs in a slide deck style. 

---

## About Page Builder

The page builder plugin was designed to help developers and designers work together with clients to deliver the best experience when building [modular/component based designs](http://alistapart.com/article/language-of-modular-design).

![typerocket-builder-plugin](https://typerocket.com/wp-content/uploads/2016/09/typerocket-builder-plugin.gif)

## Basic Usage

For the most basic setup duplicate your themes current `page.php` template and rename it to `standard.php`. Now, in the `page.php` template file use the page builder.

```php
<?php get_header();

if( tr_posts_field("use_builder") == '1') {
    tr_components_field('builder');
} else {
    get_template_part('standard');
}

get_footer();
```

This code will use the old page template `standard.php` if the builder is not being used.

## Component Folders

To start added components there are three directories you need to work with. Do not let the number of directories fool you. Using the builder is almost too easy.

The directories are: `resources/visuals/builder`, `resources/components/builder`, and `wordpress/assets/components/builder`.

The `resources/visuals/builder` directory is for adding the HTML for each components front-end design. The `resources/components/builder` directory is for adding the backend fields for each component. Finally, the `wordpress/assets/components/builder` directory is for the thumbnails of the components.

In a fresh installation of TypeRocket there is an example `content` component already included to help you get started.

## Content Component

To get started examine the content component. This will give you a deeper look at what is going on and just how easy builder modular designs can be.

### Backend Fields

First, look at the components backend `resources/visuals/builder/content.php`. Here you can see two fields have been added. One for a headline and one for some content. Also, the `<h1>` tag gives the component its name in the WordPress admin.

```php
<h1>Content Component</h1>
<?php
echo $form->text('Headline');
echo $form->editor('Content');
```

*Note: the `$form` variable is added for you and is an instance of the `\TypeRocket\Elements\Form` class.*

### Thumbnail

To make the builder admin slide resemble the look of the component you a thumbnail has been added at `wordpress/assets/components/builder/content.png`. Thumbnails can be anything you like. In many cases a screenshot of the design it implements is the way to go.

### Visual

Take a look at the content components visual. Here you can see that the fields from the backend are accessible through the `$data` variable. This is true for all visuals. 

```html
<div class="builder-content">
    <h2><?php echo esc_html($data['headline']); ?></h2>
    <hr />
    <?php echo wpautop( ($data['content'] ); ?>
</div>
```

## Adding Components

To add new components follow the pattern of the included content component. It is that easy. Simply create a visual, fields component, and thumbnail in the builder folder of each location.