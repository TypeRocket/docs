Title: Theme Templating
Description: Drop the WordPress loop and start using MVC.

---

## Getting Started

To understand the TypeRocket theme templating system lets walk through an example implementation. In this example, we will make a blog using views, template controllers, and models.

## Template Routing & Controllers

WordPress uses what is called the [template hierarchy]([https://developer.wordpress.org/themes/basics/template-hierarchy/](https://developer.wordpress.org/themes/basics/template-hierarchy/)) to route user http requests.

In WordPress, the theme templates do three things:

- Routes requests.
- Do the work of a controller.
- Display the view.

For example, if your theme has a `single-post.php` template file, all requests for a "single blog post" will be routed to that template file. The file will also control the logic flow and display the view for the page. This bundling of responsibilities is not ideal as it can lead to numerous bugs and scalability issues.

On the other hand, TypeRocket's template router seamlessly integrates into the WordPress template hierarchy pattern allowing you to use the MVC pattern while maintaining backward compatibility withing WordPress. Using the TypeRocket template router has several advantages: 

- WordPress core templates only allow for 400 error handling. The template router allows for 500, 401, 403, or any other error templates to be used.
- Takes your view out of the global scope, greatly reducing your chances of variable naming collisions, thus reducing the odds for bugs.
- Affords you the opportunity to routes requests based on the HTTP methods of `POST`, `PUT`, `GET`, or `DELETE` in an eloquent way.
- Empowers the practice of [the SOLID design principle]([https://en.wikipedia.org/wiki/SOLID](https://en.wikipedia.org/wiki/SOLID)).

Take a look at an example `single-post.php` file.

```php
<?php // themes/my-theme/single-post.php
tr_template_router(function() {
	return tr_view('blog.single');
});
```

In this example we use `tr_template_router()` to further route the request through the [TypeRocket HTTP middleware](/docs/v1/middleware/) and then to our anonymous function controller. For simplicity, we are using an anonymous function as the controller. However, using an anonymous function as the controller can be limiting.

### Making The Controller

Let's create an actual controller class and use it instead. We can quickly create our template controller using the galaxy command `make:controller`.

```
php galaxy make:controller template Blog
```

This command will create the following controller for us.

```php
<?php  
namespace App\Controllers;  
  
use \TypeRocket\Controllers\TemplateController;  
  
class BlogController extends TemplateController  
{  

}
```

### Single Post Method

Next, let's add a method to our controller called `post()`. We can use the new method as the entry point into our controller from the WordPress template and return the view we used previously.

```php
class BlogController extends Controller  
{  
    public function single()  
    {  
        return tr_view('blog.single');  
    }
}
```

Now, let's update the `single-post.php` template. Using the TypeRocket shorthand, we can quickly target our `BlogController`. 

```php
<?php // themes/my-theme/single-post.php
tr_template_router('single@Blog');
```

*Note: The `tr_template_router()` function works in the same way you might use `tr_route()->do()` with a few exceptions.*

### Single Post View

Now, in our views folder under `resources/views` create a new view structure and file as follows. `resources/views/blog/single.php`. From here, we can use the view like we would use a normal WordPress template.

```php
<?php // resources/views/blog/single.php
get_header();

if(have_posts()) :
while(have_posts()) : the_post();
	the_title('<h1>', '</h1>');
	the_content();
endwhile;
endif;

get_footer();
```

While this approach works fine, now that we are using the MVC pattern and TypeRocket views, we can dry up the template code skipping the WordPress loop, and its helper functions like `get_header()`, `the_title()`, or `the_content()` altogether. By drying up our views, we can avoid many of the issues and the clutter that often slows down development or breaks the site entirely.

Lets dry thing things up a bit using models and come back to the `single.php` view file once we have the `Post` model configured.

## Post Model

In your `app/Models` folder, locate the `Post.php` file. This file is used to define your `Post` model. Let's add two methods to the model. One method to handle the post title and the other for the post content.

```php
class Post extends WPPost  
{
    protected $postType = 'post';

	public function content()  
	{  
	    $content = get_the_content(null, false, $this->wpPost);  
		$content = apply_filters( 'the_content', $content );  
		$content = str_replace( ']]>', ']]>', $content );  
		return $content;  
	} 
  
	public function title()  
	{  
	    return get_the_title($this->wpPost());  
	}
}
```

These methods allow us to access [the WordPress template tags for the post]([https://developer.wordpress.org/themes/references/list-of-template-tags/#post-tags](https://developer.wordpress.org/themes/references/list-of-template-tags/#post-tags)). You can add as many of these as your site needs.

The main benefit of using models over the WordPress loops, in our view, will be the ability to encapsulate the logic of our views into the model. 

## Replacing The WP Loop

Now, we can begin replacing the WordPress loop and use the modern pattern of MVC. Back in the `BlogContoller` add the following constructor.

```php
class BlogController extends TemplateController  
{

	public function __construct()  
    {  
	    $this->buildPosts(\App\Models\Post::class);
    }

    // ...

}
```

This code will take the main `WP_Query` and convert it the posts into the TypeRocket `Post` model and assign the results to the controller's `$posts` property.

Next, pass the new results set to the `single.php` view.

```php
class BlogController extends Controller  
{  
    public function post()  
    {  
        return tr_view('blog.single', ['posts' => $this->posts]);  
    }
}
```

Then, in the view file, you can use a `foreach` loop. Keep in mind doing this will not allow you to use the loop helper functions in your template view. These functions include `the_title()`, `the_content()`, and others. However, you will be using the `Post` model now so you will no longer need to use the functions in the loop itself.

```php
<?php // resources/views/blog/single.php
get_header();

if($posts) : 
foreach($posts as $post) :
	echo '<h1>' . $post->title() . '</h1>';
	echo $post->content();
endwhile;
endif;

get_footer();
```

## Advanced Views

Now, many of our views would have redundant code because in each file, we will use the `get_header()` and `get_footer()`.  

```php
<?php // resources/views/blog/single.php
get_header();
// your code ...
get_footer();
``` 

Instead, we can use view layouts. Layouts allow our views to be nested inside a parent view. That parent view can be the single location where our header and footer will be located. 

Make a layout in your views folder under `layout/blog.php` like so:

```php
<?php // resources/views/layouts/blog.php
get_header();

$this->yied('main');

get_footer();
```

Now, we can tell the `single.php` view to use the blog layout as its parent. The parent layout view will print the content of the `single.php` view where it calls `yield('main')`.

```php
<?php // resources/views/blog/single.php
$this->layout('layouts.blog');

if($posts) : 
foreach($posts as $post) :
	echo '<h1>' . $post->title() . '</h1>';
	echo $post->content();
endwhile;
endif;
```

Further, we can use some special view functions so we can use views for our headers and footers too. This will allow us to avoid using `get_header()` and `get_footer()` altogether so we can use views for the header and footer instead.

Now, create two new views under `parts/header.php` and `parts/footer.php`. Then place the code you were using in the WordPress template files for these. Finally, update your layout file.

```php
<?php // resources/views/layouts/blog.php
$this->header('parts.header');

$this->yied('main');

$this->footer('parts.footer');
```



 