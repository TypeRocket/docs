Title: Post Types
Description: The post type API lets you extends the types of content available in WordPress.

---

## About Post Types

The post type API lets you extend the types of content available in WordPress.

Each type of content, or “post type”, should have a single responsibility. For example, “Post” and “Page” are post types built into WordPress and have separate responsibilities. “Post” is simply responsible for blog posts and “Page” for standard web pages.

“Post” is responsible for only one thing; blog posts. If you want to add a “Book”, type of content, to WordPress, you don’t want to cheat and add a category of “Books” to a blog post.  This will break single responsibility (in SOLID principles). Books are not blog posts. You want to create a post type called “Book” for your books.

## Getting Started

To get started with post types using TypeRocket, read these docs.

1. [Post Types: Making](/docs/v5/post-types-making/)
2. [Post Types: Theming](/docs/v5/post-types-theming/)
3. [Post Types: Secured](/docs/v5/post-types-securing/)

## Adding a Post Type

For terminology's sake, you “register” a “post type” when you want to add a new type of content. With TypeRocket Pro, you add post types without having to understand the inner-workings of WordPress. 

To register a “Book” post type in WordPress, you only need one line of code.

```php
tr_post_type('Book');
```

This one line of code adds the post type to the admin, sets all the correct labels in the navigation and applicable places, and implements the required WordPress hooks. This would typically take many lines of code.

! **Note**: you do not need to use any WordPress hook with TypeRocket here.

TypeRocket takes Post Types to the next level. Let us assign the “Book” post type to a variable to see these features.
```php
$book = tr_post_type('Book');
```

### Custom Plural Form

In some cases, you will not want TypeRocket to manage the grammar for the post types plural form.

You can set your own by supplying it as the second argument when creating the post type.

```php
tr_post_type('Book', 'Books');
```

For example, when you have other ideas altogether.

```php
tr_post_type('Person', 'Team');
``` 

## Modify Post Type

To modify an existing post type when using `tr_post_type` pass the post type ID/name as the only value. If editing a **built-in** post type, no hooks are required. 

```php
tr_post_type('post')->setIcon('book');
```

**If editing any other post type**, you need to use the `init` action and call the `register()` method.  Also, you may need to set the priority of the `init` action to a higher number for overriding to work.

```php
$priority = 11;

add_action('init', function() {
    $product = tr_post_type('product')->setIcon('book');
    $product->register();
}, $priority);
```

! **Note**: When overriding a post type created by a plugin, that plugin may have its code that TypeRocket can not override due to how that plugin works. For example, in WooCommerce you can not override the post type icon because WooCommerce implements its own CSS code and the TypeRocket CSS does not have more specificity than WooCommerce for the icon design.

### Adding custom fields to posts

With just a few lines of code, you can add the banner and subheading fields to the WordPress `post` post type using TypeRocket. All you need to do is create a [meta box](https://typerocket.com/docs/v5/meta-boxes/) and add it to the `post` screen. Then you can define a content callback and create the fields you need using the [forms API](https://typerocket.com/docs/v5/forms/).

Here you can add everything you need in your theme's `functions.php` file.

```php
<?php // functions.php
$boxPosts = tr_meta_box('Post Details');
$boxPosts->addScreen('post');
$boxPosts->setCallback(function(){
    $form = tr_form();
    echo $form->image('Banner Image');
    echo $form->text('Subheading');
});
```

### Adding custom fields to pages

If you want to add custom fields to a page, you should use the same approach. All you need to do is create a new meta box and add the screen called `page`.

```php
$boxPages = tr_meta_box('Page Details');
$boxPages->addScreen('page'); // updated
$boxPages->setCallback(function(){
    $form = tr_form();
    echo $form->image('Banner Image');
    echo $form->text('Subheading');
});
```

## Setting an Icon

To set a custom menu icon use the `setIcon()` method and a [WordPress dashicon](https://developer.wordpress.org/resource/dashicons).

```php
$book->setIcon('dashicons-book');
```

## Title Placeholder Text

By default, "Enter title here" is the placeholder text of every post type. You can change this with TypeRocket, without hooks, by using the `setTitlePlaceholder()` method.

```php
$book->setTitlePlaceholder('Enter book title here');
```

## Forms and Fields

There are 4 place to add custom content within the `<form>` element for each post type. You can open up these sections with 4 different methods: `setTitleForm()` for after the title, `setTopForm()` for before the title, `setBottomForm()` for the very bottom and `setEditorForm()` for after the editor.

Take a look at opening up a content section after the title area.

```php
$book->setTitleForm();
```

When `WP_DEBUG` is set to `true` TypeRocket shows you where the section was opened and suggests a function to be used to add content.

*Note: this applies to all four methods.*

![setTitleForm Debug](https://typerocket.com/wp-content/uploads/2015/07/docs-post-type-settitleform-debug-mode.png)

### Suggested function

By creating the suggested function, you are able to start printing content to the screen.

```php
function add_form_content_book_title() {
    echo "<h2>My Book Content</h2>";
}
```

### Callback

Alternatively, when setting a form section, you can supply an anonymous function as a callback to do the same.

Take a look at adding content after the editor using a callback.

```php
$book->setEditorForm(function() {
    echo "<h2>My Book Content</h2>";
});
```

### Fields

Adding fields to a post type can be done with the form content methods.

```php
$book->setEditorForm(function() {
    $form = tr_form();
    echo $form->text('Custom Field Name');
});
```

## Custom Labels

To customize post type labels you can use the shortcut setting `labeled`. This setting makes setting labels for translation easier for plugins to detect and for you to set.

```php
tr_post_type('Book', 'Books', [
    'labeled' => [
        __('Book'),
        __('Books'),
        false, // Keep capitalization
    ]
]);

tr_post_type('CPT', 'CPTs', [
    'labeled' => [
        __('CPT'),
        __('CPTs'),
        true, // Keep capitalization
    ]
]);
```

### Complete Override

You can also completely override the labels using the `setLabels()` method:

```php
$upperPlural = 'Books';
$upperSingular = 'Book';
$lowerSingular = 'book';
$pluralLower = 'books';

tr_post_type('Book', 'Books')->setLabels([
    'add_new'               => _x('Add New', 'post_type:book', 'your-custom-domain'),
    'all_items'             => sprintf( _x('All %s', 'post_type:book', 'your-custom-domain'), $upperPlural),
    'archives'              => sprintf( _x('%s Archives', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'add_new_item'          => sprintf( _x('Add New %s', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'attributes'            => sprintf( _x('%s Attributes', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'edit_item'             => sprintf( _x('Edit %s', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'filter_items_list'     => sprintf( _x('Filter %s list %s', 'post_type:book', 'your-custom-domain'), $pluralLower, $upperSingular),
    'insert_into_item'      => sprintf( _x('Insert into %s', 'post_type:book', 'your-custom-domain'), $lowerSingular),
    'item_published'        => sprintf( _x('%s published.', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'item_published_privately' => sprintf( _x('%s published privately.', 'your-custom-domain'), $upperSingular),
    'item_updated'          => sprintf( _x('%s updated.', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'item_reverted_to_draft'=> sprintf( _x('%s reverted to draft.', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'item_scheduled'        => sprintf( _x('%s scheduled.', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'items_list'            => sprintf( _x('%s list', 'post_type:book', 'your-custom-domain'), $upperPlural),
    'menu_name'             => sprintf( _x('%s',  'post_type:book:admin menu', 'your-custom-domain'), $upperPlural),
    'name'                  => sprintf( _x('%s', 'post_type:book:post type general name', 'your-custom-domain'), $upperPlural),
    'name_admin_bar'        => sprintf( _x('%s', 'post_type:book:add new from admin bar', 'your-custom-domain'), $upperSingular),
    'items_list_navigation' => sprintf( _x('%s list navigation', 'post_type:book', 'your-custom-domain'), $upperPlural),
    'new_item'              => sprintf( _x('New %s', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'not_found'             => sprintf( _x('No %s found', 'post_type:book', 'your-custom-domain'), $pluralLower),
    'not_found_in_trash'    => sprintf( _x('No %s found in Trash', 'post_type:book', 'your-custom-domain'), $pluralLower),
    'parent_item_colon'     => sprintf( _x("Parent %s:", 'post_type:book', 'your-custom-domain'), $upperPlural),
    'search_items'          => sprintf( _x('Search %s', 'post_type:book', 'your-custom-domain'), $upperPlural),
    'singular_name'         => sprintf( _x('%s',  'post_type:book:post type singular name', 'your-custom-domain'), $upperSingular),
    'uploaded_to_this_item' => sprintf( _x('Uploaded to this %s', 'post_type:book', 'your-custom-domain'), $lowerSingular),
    'view_item'             => sprintf( _x('View %s', 'post_type:book', 'your-custom-domain'), $upperSingular),
    'view_items'            => sprintf( _x('View %s', 'post_type:book', 'your-custom-domain'), $upperPlural),
], 'Book');
```

## Set Archive Slug

To set the slug for the custom post type and change the default use the method `setSlug()`.

```php
$book->setSlug('library');
```

Any time you add a new post type or change the slug, you need to flush the WordPress rewrite rules.

*Note: Flush rewrites by clicking "Settings > Permalinks > Save Changes".*

### Slug With Front

Sometimes you will not want your post type slug structure be prepended to the front base. For example, if your main permalink structure is `/blog/%postname%/` then your links will be `/blog/library/%postname%/` by default because `with_font` is `true` by default. To make your post type slug stop prepending to the front base URL set the `with_front` setting to `false`; the result will be `/library/%postname%/` instead of `/blog/library/%postname%/`.

```php
$withFront = false;
$book->setSlug('library', $withFront);
```

Or, you can simply disable `with_front`.

```php
$book->disableSlugWithFront();
```

## Show Or Hide Admin & Frontend

Sometimes you don't want post types to have an archive or single pages. You can use the `setAdminOnly()` method to keep a post type out of the front-end.

Take a look at making a new post type that is admin only.

```php
$storeManagers = tr_post_type('Store Manager');
$storeManagers->setAdminOnly();
```

Or, inversely you can hide the post type from the admin.

```php
$storeManagers->hideAdmin();
```

To hide the post type from the front-end.

```php
$storeManagers->hideFrontend();
```

## Edit Archive Query & Page Limit

You can set the number of posts that appear on the post types archive page using the `setArchivePostsPerPage()` method.

The `setArchivePostsPerPage()` takes one aurgument:

- `limit` - An integer `-1` for all posts and any positive number for the specific limt.

```php
$member = tr_post_type('Member');
$member->setArchivePostsPerPage(-1);
```

For more control you can use `setArchiveQueryKey($key, $value)` to control specific values:

```php
// Same as $member->setArchivePostsPerPage(-1)
$member->setArchiveQueryKey('posts_per_page', -1);
```

Also, you can remove an archive query key, but this only applies to the specific TypeRocket created post type instance.

```php
// Only applies to the TypeRocket 
// created post type
$member->removeArchiveQueryKey('posts_per_page');
``` 

## Disable Archive Page

you can disable the archive page on a post type using the `disableArchivePage()` method

```php
$member->disableArchivePage();
```

## Apply: Taxonomy and Meta Boxes

Adding `Registrable` (PostType, Taxonomy, MetaBox) object instances like a `Taxonomy` or `MetaBox` is extremely simple when using the TypeRocket post type object.

To add a `Taxonomy` to a post type instance a `Taxonomy` and then use the `apply()` method.

```php
$publisher = tr_taxonomy('Publisher');
$book->apply($publisher);
```

To add a `MetaBox` it is just as simple.

```php
$bookDetails = tr_meta_box('Book Details');
$book->apply($bookDetails);
```

### Get Applied Registrable Objects

To see what objects have been applied use the `getApplied()` method.

```php
$uses = $book->getApplied();
```

### Advanced usage of apply

You can also apply as many `Registrable` objects as you like using each as a new parameter.

```php
$publisher = tr_taxonomy('Publisher');
$bookDetails = tr_meta_box('Book Details');
$book->apply($bookDetails, $publisher);
```

Also, an array. You can decide.

```php
$publisher = tr_taxonomy('Publisher');
$bookDetails = tr_meta_box('Book Details');
$book->apply( [$bookDetails, $publisher] );
```
### Applying to Existing Taxonomies

To apply a taxonomy that already exists use the `addTaxonomy()` method and pass the taxonomies ID as a parameter.

```php
$book->addTaxonomy('category');
$book->addTaxonomy('post_tag');
```

## Setting and Getting the ID

The ID is used to specify the name that WordPress associates with your post types. It is also registered under the ID. When you register "Book" with TypeRocket, the id is set to "book".

You can change the ID using the `setId()` method.

```php
$book->setId('library_book');
```

Get the ID

```php
$bookId = $book->getId();
```

## Arguments

There are five methods for dealing with arguments. Arguments are used when the post type is being registered. All arguments can be [found in the WordPress codex](https://codex.wordpress.org/Function_Reference/register_post_type#Arguments).

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without affecting the already set values.

```php
$args = $book->getArguments();
$args = array_merge( $args, [ 'public' => true ] );
$book->setArguments( $args );
$public = $book->getArgument( 'public' );
$book->removeArgument( 'public' );
$book->setArgument( 'public', $public );
```

## Add Table Columns

You can add and remove columns from the admin post type index page table using TypeRocket. Here we will use a post type of person for our examples. Also, we will have one custom field so we can add it in a custom table column.

```php
$person = tr_post_type('Person', 'People');
$person->setTitleForm(function() {
    $form = tr_form();
    echo $form->text('Job Title');
    echo $form->image('Photo');
});
```

### Add Column

To add a column it needs to match the custom field's name.

```php
$person->addColumn('Job Title');
```

*If the custom field is grouped then you need to use the base group name.*

The `addColum()` method takes up to 4 aurguments.

1. `field` - The name of the field to add. You can use any string format but lowercase letters and `_` (underscores) should be used for precision. 
2. `sort_by` - The column is sortable `true` or `false`.  Doubles as order_by when string value is passed. String options include: `int`, `double`,  `date`, `datetime`,  `time`, and `str`.
3. `label` - The tabled header column label.
4. `callback` - Return the value to be displayed in the column.

Here is an advanced example:

```php
$person->addColumn('Photo', false, 'Photo', function($value) {
    return wp_get_attachment_image($value, 'thumbnail');
});
```

### Remove Column

You can remove a column using the `removeColumn()` method. The `removeColumn()` method takes 1 `string` argument and it needs to match the field's name.

```php
$person->removeColumn('date');
```

### Set Primary Column

In some cases, you will not have a title column and still want the "Edit | Quick Edit | Trash | View" controls to be used. To do this, you will need to set a new primary column.

```php
$person->setPrimaryColumn('job_title');
```

## REST API

You can also set the WordPress post types REST API resource location using the `setRest()` method. 

```php
$team->setRest('people');
```

## Forcing Root URLs

The standard rewrite rules for custom post types is `/{post-type}/{post-name}`. However, you can set a post types custom URL to the site's root by using the `setRootOnly()` method; doing this will change the post types URLs from `/{post-type}/{post-name}` to `/{post-name}`. Further, TypeRocket makes sure URLs are not duplicated for any other post type using the site's root.

- Disabled the archive page.
- Forces the use of the site's root.
- Forces a unique `post_name` for URLs.

```php
$team->setRootOnly();
```

## Gutenburg

Force disable or enable Gutenberg for a post type use the `enableGutenberg()` or `forceDisableGutenberg()` methods. 

If you have edited your `app.php` config file with your own custom Gutenberg feature settings your config settings will take priority over `enableGutenberg()` and `forceDisableGutenberg()`. To take advantage of these new features, be sure to keep your Gutenberg config feature set to true.

### Enable

```php
$team->enableGutenberg();
```

### Disable

```php
$team->forceDisableGutenberg();
```

## Add Support

Add support for a WordPress feature:

- `title`: The post's title.
- `editor`: Gutenburg or the classic editor.
- `author`: Author metabox.
- `thumbnail`: Add to get the featured image; the current theme must also support post-thumbnails.
- `excerpt`: Excerpt metabox.
- `trackbacks`: Trackbacks metabox.
- `custom-fields`: WordPress powered custom fields metabox; not required for TypeRocket custom fields.
- `comments`: Enabled comments.
- `revisions`: Save revisions.
- `page-attributes`: Adds menu order; optionally, hierarchical must be true to show parent select box.
- `post-formats`: Add post formats.

```php
$symbol = tr_post_type('Symbol');
$symbol->addSupport('comments');
```

You can also override all the support features by using `setSupports($array)`:

```php
$symbol->setSupports(['title', 'editor', 'comments']);
```

### Remove All Features

To remove all features from a post type use the `featureless()` method:

```php
$symbol->featureless();
```

## Set Hierarchical

To make a post type hierarchicle:

```php
$species = tr_post_type('Species');
$species->setHierarchical(true);
```

## Exclude From Search

To exclude a post type from search:

```php
$code = tr_post_type('Code');
$code->excludeFromSearch(true);
```

## Delete With User

To delete a user's posts in a post type when that user is deleted:

```php
$code = tr_post_type('Code');
$code->deleteWithUser(true);
```

## Menu Position

To set the post type menu position:

```php
$code = tr_post_type('Code');
$code->setPosition(25);
```

The number can be from 5 to 100.

## Custom Capabilities

If you would like to set custom capabilities for a post type, use the `customCapabilities()` method. This method will replace the post type's capability settings with the singular and plural post type name.

```php
$product = tr_post_type('Product');
$product->customCapabilities();

// Capabilities will become:
//
// edit_post -> edit_product
// read_post -> read_product
// edit_posts -> edit_products
// ...
```

Once a post type has custom capabilities, you will need to apply those capabilities to a role. Further, roles should only be updated on plugin or theme (de)activation.

```php
// After theme/plugin (de)activation
$caps = tr_roles()->getCustomPostTypeCapabilities('product', 'products');
tr_roles()->updateRolesCapabilities('administrator', $caps);
```

## Registration Hook: Action

WordPress gives you a hook when a post type is registered. This makes registering post types developer-friendly. This hook exists to let plugin developers limit post type conflicts.

You can modify the registration if you need to from here.

```php
add_action('registered_post_type', function($postType, $args) {}, 10, 2);
```

## Set Handler & Controller

By default, the controller for a post type is automatically set to one using its shared name in the `App` namespace. To set a custom controller for a post type use `setHandler()`.

```php
$product = tr_post_type('Product');
$product->setHandler(\MyCustomNamspace\Controllers\ProductController::class);
```

## Set Model Class

By default, the model class for a post type is automatically set to one using its shared name in the `App` namespace. To set a custom model class for a post type use `setModelClass()`. When setting the model a `tr_form()` will locate that model for the post types custom fields.

```php
$product = tr_post_type('Product');
$product->setModelClass(\MyCustomNamspace\Models\Product::class);
```
