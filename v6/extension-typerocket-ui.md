Title: Extension: TypeRocket UI
Description: Quickly add post types and Taxonomies to your site.

---

## TypeRocket UI

If you need to quickly add a post type or taxonomies to your WordPress site, you can use the TypeRocket UI extension.

This extension is designed to make creating post types, taxonomies, and meta boxes fast while providing robust customization tools. 

This tool is beneficial if you do not want to get into coding your post types. However, the post type or taxonomies created by this tool will be stored in database so it is always recommended that you have all of your post types, taxonomies, and fields coded so that end users cannot break your site.

You can also move the menu location of the Post Types UI to another location by setting the `TYPEROCKET_UI_MENU` constant in your `wp-config.php` file.

```php
// Add to the tools menu
define('TYPEROCKET_UI_MENU', 'tools');

// Add to the setings menu
define('TYPEROCKET_UI_MENU', 'settings');
```
