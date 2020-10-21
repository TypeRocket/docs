Title: Post Types: Looping
Description: Set a specific number of posts to show per post type archive.

---

## The Main Loop

"The Main Loop" is responsible for loading the initial posts in your theme. This means the main loop controls how many posts are shown on the blog archive and in a custom post type archive. If you want to edit the number of posts in the main loop you need to use the [pre_get_posts](https://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts) hook.

Below you tell WordPress to show a 15 posts on the blog archive page and 20 for a "team" post type archive page.

Here is the code you would add to your `functions.php` file to edit the main WordPress loop.

```php
<?php // functions.php
add_action( 'pre_get_posts', function( $loop ) {

    if ( is_admin() || ! $loop->is_main_query() ) {
        // do nothing if in the admin or not in the main loop
        return;
    }

    if ( is_home() ) {
        // Show 15 posts for the blog archive
        $loop->set( 'posts_per_page', 15 );
        return;
    }

    if ( is_post_type_archive( 'team' ) ) {
        // Show 20 posts for the custom post type 'team'
        $loop->set( 'posts_per_page', 20 );
        return;
    }

}, 1);
```