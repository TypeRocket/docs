Title: Post Types
Description: Registering, adding and extending your post types and much more.

---

The post type API lets you extends the types of content available in WordPress.

Each type of content, or “post type”, should have a single responsibility. For example, “Post” and “Page” are post types built into WordPress and have separate responsibilities. “Post” is singly responsible for blog posts and “Page” for standard web pages.

“Post” is responsible for only one thing; blog posts. If you want to add a “Book”, type of content, to WordPress you don’t want to cheat and add a category of “Books” to a blog post.  This will break single responsibility. Books are not blog posts. You want to create a post type called “Book” for your books.

## Adding a Post Type

For terminologies sake you “register” a “post type” when you want to add a new type of content. With TypeRocket you add post types without having to understand the inner-workings of WordPress. 

To register a “Book” post type in WordPress you only need one line of code.

```php
<?php // functions.php
include 'typerocket/init.php';

tr_post_type('Book');
```

This one line of code adds the post type to the admin, sets all the correct labels in the navigation and applicable places, and implements the required WordPress hooks. This would normally take many many lines of code.

*Note: you do not need to use any WordPress hook with TypeRocket here.*

TypeRocket takes Post Types to the next level. When you create a post type with TypeRocket you are instancing a special PHP object that can do much more.

Let's assign the “Book” post type to a variable to see these features.

```php
<?php // functions.php
include 'typerocket/init.php';

$book = tr_post_type('Book');
```

### Custom plural form

In some cases, you will not want TypeRocket to manage the grammar for the post types plural form.

You can set your own by supplying it as the second argument when creating the post type.

```php
tr_post_type('Book', 'Books');
```

For example when you have other ideas altogether.

```php
tr_post_type('Person', 'Team');
``` 

## Setting an Icon

To set a custom menu icon use the `setIcon()` method.

```php
$book->setIcon('Book');
```

*Note: Browse the “dev” TypeRocket plugin page in the admin under the "Icons" tab to see the list of options.*

## Title Placeholder Text

By default "Enter title here" is the placeholder text of every post type. You can change this with TypeRocket, without hooks, by using the `setTitlePlaceholder()` method.

```php
$book->setTitlePlaceholder( 'Enter book title here' );
```

## Form Content

There are 4 place to add custom content within the `<form>` element for each post type. You can open up these sections with 4 different methods: `setTitleFrom()` for after the title, `setTopFrom()` for before the title, `setBottomFrom()` for the very bottom and `setEditorFrom()` for after the editor.

Take a look at opening up a content section after the title area.

```php
$book->setTitleFrom();
```

When `TR_DEBUG` is set to `true`, as it is by default, TypeRocket shows you where the section was opened and suggests a function to be used to add content.

*Note: this applies to all 4 methods.*

![setTitleForm Debug](https://l.rb.typerocket.test/wp-content/uploads/2015/07/docs-post-type-settitleform-debug-mode.png)

### Suggested function

By creating the suggested function you are able to start printing content to the screen.

```php
function add_form_content_book_title() {
    echo "<h2>My Book Content</h2>";
}
```

### Callback

Alternatively, when setting a form section you can supply an anonymous function as a callback to do the same.

Take a look at adding content after the editor using a callback.

```php
$book->setEditorForm(function() {
    echo "<h2>My Book Content</h2>";
});
```

## Set Archive Slug

To set the slug for the custom post type and change the default use the method `setSlug()`.

```php
$book->setSlug('library');
```

Any time you add a new post type or change the slug you need to flush the WordPress rewrite rules.

*Note: Flush rewrites by clicking "Settings > Permalinks > Save Changes".*

## Setting Admin Only

Sometimes you don't want post types to have an archive or single pages. You can use the `setAdminOnly()` method to keep a post type out of the front-end.

Take a look at making a new post type that is admin only.

```php
tr_post_type('Store Manager')->setAdminOnly();
```

## Apply: Taxonomy and Meta boxes

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
$book->apply( array($bookDetails, $publisher) );
```

## Setting and Getting the ID

The ID is used to specify the name that WordPress associates with your post types. It is also registered under the ID. When you register "Book" with TypeRocket the id is set to "book".

You can change the ID using the `setId()` method.

```php
$book->setId('library_book');
```

Get the ID

```php
$bookId = $book->getId();
```

## Arguments

There are 5 methods for dealing with arguments. Arguments are used when the post type is being registered. All arguments can be [found in the WordPress codex](https://codex.wordpress.org/Function_Reference/register_post_type#Arguments).

These methods are: `getArguments()`, `setArguments()`, `getArgument()`, `setArgument()` and `removeArgument()`.

1. `getArguments()` returns the full array of arguments.
2. `setArguments()` takes an array of arguments.
3. `getArgument()` return an argument by its key.
4. `setArgument()` sets an argument by a key.
5. `removeArgument()` removes an argument by its key.

Take a look at using all the methods without effecting the already set values.

```php
$args = $book->getArguments();
$args = array_merge( $args, array( 'public' => true ) );
$book->setArguments( $args );
$public = $book->getArgument( 'public' );
$book->removeArgument( 'public' );
$book->setArgument( 'public', $public );
```

## Registration Hook: Action

WordPress gives you a hook when a post type is registered. This makes registering post types developer friendly. This hook exists to let plugin developers limit post type conflicts.

You can modify the registration if you need to from here.

```php
add_action('registered_post_type', function($postType, $args) {}, 10, 2);
```