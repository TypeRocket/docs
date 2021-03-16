Title: Extension: Post Types UI
Description: Quickly add post types and Taxonomies to your site.

---

## Post Types UI

If you need to quickly add a post type or taxonomies to your WordPress site, you can use the Post Types UI extension.

The Post Types UI extension adds an admin menu named "Registrables".

This extension is designed to make creating post types, taxonomies, and meta boxes fast while providing robust customization tools. 

This tool is beneficial if you do not want to get into coding your post types. However, it is always recommended that you have all of your post types, taxonomies, and fields coded so that end users can not break your site.

You can also move the menu location of the Post Types UI to another location by setting the `TR_POST_TYPES_MENU` constant in your `wp-config.php` file.

```php
// Add to the tools menu
define('TR_POST_TYPES_MENU', 'tools');

// Add to the setings menu
define('TR_POST_TYPES_MENU', 'settings');
```