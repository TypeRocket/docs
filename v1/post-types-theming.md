Title: Post Types: Theming
Description: Learn how to take custom fields and apply them to theme templates.

---

## Getting Setup

Adding custom fields to a post type is easy with TypeRocket Pro, and theming with them is too. In our [example of a team post type](/docs/v1/post-types-making/) we walked through a basic setup. Now, you need to add that data to the theme.

Before getting started with templating, make sure the previous example's post type `person` is in your theme's `functions.php` file:

```php
$team = tr_post_type('Person', 'Team');
$team->setIcon('dashicons-groups');
$team->setSupports(['title']);
$team->addColumn('Job Title');
$team->setTitlePlaceholder( 'Enter full name here' );
$team->setRest();
$team->setArchivePostsPerPage(-1);
$team->setTitleForm( function() {
    $form = tr_form();
    echo $form->image('Photo');
    echo $form->editor('post_content')->setLabel('About Person');
});

tr_meta_box('Team Details')->apply($team)->setCallback(function() {
    $form = tr_form();
    echo $form->text('Job Title');
});

tr_taxonomy('Department')->apply($team);
```

Next, flush the rewrite rules in WordPress. This **VERY IMPORTANT**. Without flushing, rewrite rules, WordPress will not have a URL for the `person` post type.

*Note: Flush rewrites by clicking Settings > Permalinks > Save Changes*

## The Files

You will need three template files when theming in our example.

- `single-person.php` for the individual `person` members.
- `archive-person.php` for the `person` archive page.
- `taxonomy-department.php` for the custom taxonomy `department`.

## Publish Content

Publish some content in the `person` post type. Notice, when you publish content to a post type with TypeRocket, the "flash message" corresponds to the post type's singular label. This helps make the management experience feel very refined.

*Note: If the post type is not public or "Admin Only" the view link is removed from the "flash message".*

## Archive Template 

Copy the dev mode code hint from the post type edit page for "Job Title" into a loop in the `archive-person.php` file.

![docs-v1-post-type-templating-code-hint](https://typerocket.com/wp-content/uploads/2020/01/docs-v1-post-type-templating-code-hint.png)

*Note: In the config.php file set `TR_DEBUG` to `true` to see the code hints.*

The code hint in this case is:

```php
tr_post_field("job_title");
```

Lets set the text for the full name and job title in the archives page loop.

Remember the full name is `the_title()`.

```php
<?php // archive-person.php
get_header(); ?>
<section id="content">
    <?php while (have_posts()) : the_post() ?>
        <h1>
            <a href="<?php the_permalink(); ?>">
                <?php the_title(); ?>
            </a>
        </h1>
        <section>
            <?php echo tr_post_field( "job_title" ); ?>
        </section>
    <?php endwhile; ?>
</section>
<?php get_footer(); ?>

```

## Single Template

Here things are getting a little more complex since we are using an image. The code hint for the image field includes a field modifier. the modifier is `:img:full:` and comes before the field name `tr_post_field(':img:full:photo')`.

The modifier takes the attachment ID returned by the image field add passes the ID to the WordPress function `wp_get_attachment_image()`. Next, the modifier uses `full` as the image size.

Take a look at the code you would need to write without the modifier:

```php
// Without modifier
$img_id = tr_post_field('photo');
echo wp_get_attachment_image((int) $img_id, 'full');

// With modifier
echo tr_post_field(':img:full:photo');
```

Now, look at everything all together:

```php
<?php // single-person.php
get_header(); ?>
<section id="content">
    <?php while (have_posts()) : the_post() ?>
        <h1><?php the_title(); ?></h1>
        <section>
            <p><?php echo tr_post_field("job_title"); ?></p>
            <?php
            echo tr_post_field(':img:full:photo');
            the_content();
            echo get_the_term_list( get_the_ID(), 'department', 'Department: ', ', ', '' );
            ?>
        </section>
    <?php endwhile; ?>
</section>
<?php get_footer(); ?>
```

Note, you will list the terms for the department with `get_the_term_list()`.

## Taxonomy Template

To keep the code simple, you will require the code from the `archives-person.php` template.

```php
<?php // taxonomy-department.php
get_template_part('archive', 'person');
```