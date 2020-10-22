Title: SEO Extension
Description: Add SEO to your post types or custom resources.

---

## Add Theme Support

To begin, be sure your theme has title tag support enabled.

```php
add_theme_support( 'title-tag' );
```

If you want to disabled the SEO extension, you can remove it from the extensions list or add the following to your `wp-config.php`.

```php
define('TR_SEO', false);
```

If Yoast SEO or All in One SEO Pack are installed the TypeRocket's SEO extension will get disabled automatically.

## SEO Meta Box

The `Seo` extension adds SEO meta boxes to all public post types by default. The meta box can be used to add:

1. Basic metadata.
2. Twitter cards.
3. Open graph tags for Facebook or Pinterest.
4. Robot meta and redirects.

The meta box also provides buttons for quickly analyzing your posts [Schema data](https://search.google.com/structured-data/testing-tool/u/0/) and [Google Page Speed](https://developers.google.com/speed/pagespeed/insights/). 


## SEO Fields For Custom Resource

You can also add the SEO field to any custom resource you like using the `tr_seo_meta_fields()` function.  For the model used, you must save the SEO fields in a column named `seo` in the database.

For example, your controller and views:

```php
// ...
use \App\Models\Node;

class NodeController extends Controller  
{  
    public function add() {
		$form = tr_from(Node::class);
		return tr_view('nodes.show', campact('form'))
	}
	
    public function create() {
		// Do some node creation
	}
	
	public function show($id) {
		$node = (new Node)->find($id);
		$url = site_url('/nodes/' . $id);
		return tr_view('nodes.show', campact('node'))->setSeoMeta($node->seo, $url);
	}   
}
```

```php
<!-- View file: nodes.add -->
<?php echo tr_seo_meta_fields($form); ?>
```

```php
<!-- View file: nodes.show -->
<?php var_dump($node); ?>
```

## SEO Hooks

The SEO extension comes with several hooks.

1. Action - `tr_seo_meta` - Used to output custom HTML meta tags.
2. Filter - `tr_seo_meta` - Used to filter the meta data used for meta tags.
3. Action - `tr_seo_fields` - Used to add custom fields to the general tab of the SEO meta box.
4. Filter - `tr_seo_post_types` - Used to control which post types have the SEO meta box.
6. Filter - `tr_seo_url` - Used to control the URL used for OG. 
 
### tr_seo_post_types

If you want to control what post types the meta box is added too, you can use the `tr_seo_post_types` filter hook. Only post types added will have the meta box added.

```php
add_filter('tr_seo_post_types', function($types) {
	return ['page', 'post'];
});
```

### tr_seo_fields

If you want to add fields to the SEO metabox, use the `tr_seo_fields` action hook.

```php
add_action('tr_seo_fields', function($form) {
    echo $form->text('basic.Keywords');
});
```

### tr_seo_meta

If you want to add fields to the SEO metabox, you will also want to display them on each page. You can use the `tr_seo_meta` action hook to output new meta.

```php
add_action('tr_seo_meta', function($seo) {
    $keywords = esc_attr($seo->meta['basic']['keywords'] ?? '')
    echo "<meta name=\"keywords\" content"{$keywords}">";
});
```