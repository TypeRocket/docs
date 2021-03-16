Title: Post Types: Theming
Description: Learn how to take custom fields and apply them to theme templates.

---

Adding custom fields to a post type is easy with TypeRocket 4.0 and theming with them is too. In our [example of a team post type](/docs/v4/post-types-making/) we walked through a basic setup. Now add that data to the theme.

## Getting Setup

To template and theme the example post type, `tr_team`.

In `functions.php`.

```php
<?php // functions.php
include('typerocket/init.php');

$team = tr_post_type('Person', 'Team');
$team->setId('tr_team');
$team->setIcon('users');
$team->setArgument('supports', ['title'] );
tr_meta_box('Team Details')->apply($team);
tr_taxonomy('Department')->apply($team);

function add_meta_content_team_details() {
    $form = tr_form();
    echo $form->text('Job Title');
}

$team->setTitlePlaceholder( 'Enter full name here' );

$team->setTitleForm( function() {
    $form = tr_form();
    echo $form->image('Photo');
    $editor = $form->editor('post_content');
    echo $editor->setLabel('About Person');
} );

```

Next, flush the rewrite rules in WordPress. This VERY IMPORTANT. Without flushing rewrite rules WordPress will not have a URL for the `tr_team` post type.

*Note: Flush rewrites by clicking Settings > Permalinks > Save Changes*

## The Files

You will need three files when theming in our example.

- `single-tr_team.php` for the individual `tr_team` members.
- `archive-tr_team.php` for the `tr_team` archive page.
- `taxonomy-department.php` for the custom taxonomy `department`.

If you did not set a custom ID, like with the taxonomy, remember that TypeRocket uses the singular name as the `ID` by default. That is why you name the taxonomy template file `taxonomy-department.php`.

This also means that you would use `single-person.php` and `archive-person.php` if you did not set the ID for the post type.

## Publish Content

Publish some content in the `tr_team` post type.

Notice when you publish content to a post type with TypeRocket the "flash message" corresponds to the post type's singular label. This helps make the management experience feel very refined.

*Note: If the post type is not public or "Admin Only" the view link is removed from the "flash message".*

## Archive Template 

Copy the dev mode code hint from the post type edit page for "Job Title" into a loop in the `archive-tr_team.php` file.

*Note: In the config.php file set TR_DEBUG to true.*

The code hint in this case is:

```php
tr_posts_field("job_title");
```
Lets set the text for the full name and job title in the archives page loop.

Remember the full name is `the_title()`.

```php
<?php // archive-tr_team.php
get_header(); ?>
<section id="content">
    <?php while (have_posts()) : the_post() ?>
        <h1>
            <a href="<?php the_permalink(); ?>">
                <?php the_title(); ?>
            </a>
        </h1>
        <section>
            <?php echo tr_posts_field( "job_title" ); ?>
        </section>
    <?php endwhile; ?>
</section>
<?php get_footer(); ?>

```

## Single Template

Here things getting a little more complex since we are using an image.

Image fields return the attachment ID since it is the best approach; not the url or an image tag. Use `wp_get_attachment_image()`.

You will also list the terms for the department `get_the_term_list()`.

```php
<?php // single-tr_team.php
get_header(); ?>
<section id="content">
    <?php while (have_posts()) : the_post() ?>
        <h1><?php the_title(); ?></h1>
        <section>
            <p><?php echo tr_posts_field("job_title"); ?></p>
            <?php
            echo wp_get_attachment_image(tr_posts_field("photo"));
            the_content();
            echo get_the_term_list( get_the_ID(), 'department', 'Department: ', ', ', '' );
            ?>
        </section>
    <?php endwhile; ?>
</section>
<?php get_footer(); ?>

```

## Taxonomy Template

To keep the code simple you will just require the code from the `archives-tr_team.php` template.

```php
<?php // taxonomy-department.php
get_template_part('archive', 'tr_team');
```

## New Quick Fields

When using custom fields in your templates using the field getter functions like `tr_posts_field` you can specify how you want to mutate that data before it is returned.

For example, before you would need the following code to create an image tag in your template:

```php
$media_id = tr_posts_field('field_name');
echo wp_get_attachment_image(media_id, 'full');
```

Now you can do it with 1 line of code:

```php
echo tr_posts_field(':img:full:field_name');
```