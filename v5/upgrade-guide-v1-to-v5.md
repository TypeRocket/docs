Title: Upgrade Guide v1 to v5
Description: Upgrading TypeRocket v1 to v5

---
## Upgrading From v1

If you are coming from TypeRocket Pro v1 upgrading should take 1 - 3 hours depending on your project size.

### Upgrading From v4

This guide is not for upgrading from v4 to v5. There is no official guide for that upgrade. The update is just that big. **TypeRocket Pro v5 Andromeda** is not fully backward compatible with **TypeRocket v4**. You will need to make a number of updates to get TypeRocket v4 to work. Because Typerocket v5 is such a big update, it is best to create a new TypeRocket v5 project and then migrate your existing v4 sites into TypeRocket v5.

## Start

First, you will need to [create a new project](/docs/v5/get-started/) by starting a clean installation. From the new installation you can copy the majority of your `app`, `database`, `storage`, `routes`, and `resources` code into the new project. However, keep in ind the following changes.

### Pro Extension Publishing: Composer Only

Previously in Typerocket Pro v1 all features were included in the base [composer](https://getcomposer.org/) installation. TypeRocket v5  has two systems instead of one: a free core system, and a Pro paid system. 

You will need to extend the free system with Pro by requiring `typerocket/professional` with composer. Then you will need to publish the professional package after installing it with `composer`. The details on this process can be found in [the composer installation guide for v5](/docs/v5/install-via-composer/).

*Note: For root installations the `typerocket.php` MU plugin is no longer needed and has been removed.*

### New Config Files

Take note of [the new config files on the typerocket/typerocket repository](https://github.com/TypeRocket/typerocket/tree/main/config). Merge any changes you have made to work with the new functions and settings. Also, be aware that `immutable()` has been replaced by `typerocket_env()`.

### Removed Non-prefixed Helper Functions

All non-prefixed helper functions have been removed like `dd`, `dots_walk`, `class_names`, and `str_contains`. Also, number of helper functions have been removed. These functions have been removed to bring TypeRocket into better compliance with the WordPress guidelines.

Some functions have been removed while other have been moved.

- `dd()` has been removed. Use [Symfony VarDumper component instead](https://symfony.com/doc/current/components/var_dumper.html) 
- `array_reduce_allowed_str()` removed.
- `database()` removed.
- `immutable()` moved to `typerocket_env()`.
- `dots_walk()` moved to `\TypeRocket\Utility\Data::walk()`.
- `dots_set()` moved to `\TypeRocket\Utility\Arr::set()`.
- `tr_cast()` moved to `\TypeRocket\Utility\Data::cast()`.
- `class_names()` moved to `\TypeRocket\Utility\Str::classNames()`.
- `str_contains()` moved to `\TypeRocket\Utility\Str::contains()`.
- `str_starts()` moved to `\TypeRocket\Utility\Str::starts()`.
- `str_ends()` moved to `\TypeRocket\Utility\Str::ends()`.
- `not_blank_string()` moved to `\TypeRocket\Utility\Str::notBlank()`.
- `mb_ucwords()` moved to `\TypeRocket\Utility\Str::uppercaseWords()`.

### Actions and Filters

All `tr_` hooks are now prefixed with `typerocket_`. This was done to bring TypeRocket into better compliance with the WordPress guidelines. 

### Helper Functions Moved

The `tr_` helper functions have been moved to the project root. See your `composer.json` file and look for the `helpers.php` file. However, the Pro version helpers are not included in the `helpers.php` file. Pro helpers are loaded from the `typerocket/professional` package.

### TypeRocketPro Namespace

There is now a `TypeRocketPro` namespace where `TypeRocket` was used. To see the full list of classes now using the new namespace look under `vendor/typerocket/professional/src`. Example classes that have been moved are:

- `\TypeRocket\Utility\Http` is now `\TypeRocketPro\Utility\Http`.
- `\TypeRocket\Extensions\Seo` is now `\TypeRocketPro\Extensions\Seo`.
- `\TypeRocket\Extensions\ThemeOptions` is now `\TypeRocketPro\Extensions\ThemeOptions`.
- `\TypeRocket\Elements\Table` is now `\TypeRocketPro\Elements\Table`.
- `\TypeRocket\Template\TwigTemplateEngine` is now `\TypeRocketPro\Template\TwigTemplateEngine`.
- And more...

### New Form System

The `tr_form()` function now loads the `App/Elements/Form` class instead of `Typerocket/Elements/Form`. `Typerocket/Elements/Form` is now named `Typerocket/Elements/BaseForm`. By moving the main form class to `App/Elements/Form` your will be able to customize how forms work in your project without any extra configuration.

### Builder & Matrix Component

One of the biggest changes to TypeRocket in v5 is the all new component system. The new component system effects the builder and matrix fields. If you are not using these fields the new component system will not affect you.

Components no longer use the folder system. The new system is PHP class based. The old folder system as built upon: 

1. **Visuals** under `resources/visuals`.
2. **Admin Components** under `resources/components`.
3. **Component Thumbnails** under `wordpress/assets/components`.

The folders `resources/visuals` and `resources/components` have been replace with the new `\TypeRocket\Template\Component` class and `config/components.php` registration system. As for thumbnails, they now live under `wordpress/assets/components` folder and must have a unique names (you no longer need to add thumbnails into *group* folders). You can control the name of the thumbnail files from the `\TypeRocket\Template\Component` class.

Component classes live under the `app/Components` folder and must be registered to the `config/components.php` file. To migrate your components from the folder system use the galaxy command to create replacement component classes and register them. you can do this with the following command:

```
php galaxy make:component <key> [<class> [<title>]]
```

*Note: If you are migrating from v1 you will want your key to match the name of your old component file name.*

For example, lets say you are migrating an existing "Image" folder system component. First, make a new "Image" component with the command:

```
php galaxy make:component image ImageComponent "Image Component"
```

This command will create your new component class at `app/Components/ImageComponent.php` and register it to `config/components.php` at the same time. 

Within the new component class, move your **Admin Component** field code into the `fields()` method and your visuals into the `render()` method.

```php
/**
 * Admin Fields
 */
public function fields()
{
    $form = $this->form();

    echo $form->image('Featured Image');
}
```

```php
/**
 * Render
 *
 * @var array $data component fields
 * @var array $info name, item_id, model, first_item, last_item, component_id, hash
 */
public function render(array $data, array $info)
{
    ?>
    <div class="builder-image">
        <?php echo wp_get_attachment_image($data['featured_image']); ?>
    </div>
    <?php
}
```

You may want to use a view for your component rendering instead of placing your template code into the component class. Using views makes the migration process very simple, because you can copy all of your existing visuals into your views folder and access them from there.

```php
/**
 * Render
 *
 * @var array $data component fields
 * @var array $info name, item_id, model, first_item, last_item, component_id, hash
 */
public function render(array $data, array $info)
{
    tr_view('visuals.builder.image', array_merge(compact('data'), $info))->render();
}
```

Finally, components need to be assigned to a group(s) in the new `config/components.php` file.

```php
<?php
return [
    /*
    |--------------------------------------------------------------------------
    | Component Registry
    |--------------------------------------------------------------------------
    */
    'registry' => [
        'image' => \App\Components\ImageComponent::class,
    ],

    /*
    |--------------------------------------------------------------------------
    | Builder
    |--------------------------------------------------------------------------
    |
    | List of components you want included for the builder group.
    |
    */
    'builder' => [
        'image',
    ],
];
```

Now, when a builder or matrix field uses the name `builder` it will use all the components listed under the `builder` group. Groups reference a component by its registry key. In the case of the `\App\Components\ImageComponent` component the key is `image`.

```php
$form = tr_form();
echo $form->builder('builder');
```

You can have multiple groups. You can scope components to a specific group by registering the component with the name of the group followed by a colon `:`. Scoping is only needed is you are migrating from the folder system to the class system, but you have component naming collisions.

```php
<?php
return [
    /*
    |--------------------------------------------------------------------------
    | Component Registry
    |--------------------------------------------------------------------------
    */
    'registry' => [
        'footer:content' => \App\Components\FooterContent::class,
        'columns:gallery' => \App\Components\ColumnsGallery::class,
        'columns:content' => \App\Components\ColumnsContent::class,
        'video' => \App\Components\Video::class,
        'gallery' => \App\Components\Gallery::class,
        'content' => \App\Components\Content::class,
        'links' => \App\Components\Links::class,
    ],

    /*
    |--------------------------------------------------------------------------
    | Builder
    |--------------------------------------------------------------------------
    |
    | List of components you want included for the builder group.
    |
    */
    'builder' => [
        'gallery',
        'content',
    ],

    /*
    |--------------------------------------------------------------------------
    | Columns
    |--------------------------------------------------------------------------
    |
    | List of components you want included for the columns group.
    |
    */
    'columns' => [
        'columns:content',
        'columns:gallery',
    ],
  
    /*
    |--------------------------------------------------------------------------
    | Columns
    |--------------------------------------------------------------------------
    |
    | List of components you want included for the footer group.
    |
    */
    'footer' => [
        'links',
        'footer:content',
    ]
];
```

Now, you can access your groups by naming your fields after them.

```php
$form = tr_form();
echo $form->builder('columns');
echo $form->matrix('footer');
echo $form->builder('builder');
```

*Note: Dynamic component thumbnails have been updated to no longer required the hook `tr_builder_component_thumbnails`. You can now control the thumbnail through the component class.*

### Icons Removed

The TypeRocket icons `\TypeRocket\Elements\Icons` have been removed in favor of [the WordPress built-in options](https://developer.wordpress.org/resource/dashicons/). To upgrade your icons replace your icon names with the WordPress Dashicons' names.

This change is due to the number of WordPress icons increasing, and the performance gains for not loading the extra icons.

### Post Types and Taxonomies

Post type and taxonomy `Model` classes now require the use a `const` for their type declaration instead of a `protected` property: `POST_TYPE` and `TAXONOMY`.

```php
class Page extends WPPost
{
    public const POST_TYPE = 'page';
}

class Tag extends WPTerm
{
    public const TAXONOMY = 'post_tag';
}
```

### Galaxy CLI Timing

The Galaxy CLI now runs during the WordPress loading process instead of running at the end of loading. The commands run during the `after_setup_theme` WordPress action.

### Application Timing

The Application Kernel now loads at a more consistent runtime. The possibility of this change impacting your business logic is low. The new timing is during the `after_setup_theme` WordPress action.

### Tachyon Template Engine

The `\TypeRocket\Template\TemplateEngine` from Pro v1 is now `\TypeRocketPro\Template\TachyonTemplateEngine`. You will need to update your configuration under `app.templates.views` from `\TypeRocket\Template\TemplateEngine` to `\TypeRocketPro\Template\TachyonTemplateEngine`.

### Views

The view system has been reduced to just one folder named `views` located at `resources/views`. In v1 admin views were located under `resources/pages`; this is no longer the case. You will need to migrate all of your admin views to the views folder. Or, if you are familiar with view contexts you can use that instead.

### New Injector Class

The `\TypeRocket\Core\Injector` class has been renamed to `\TypeRocket\Core\Container`.

### Root Theme

The root theme no longer limits you to just itself. You can use other themes installed under `wp-content/themes` now.

### Extensions

Extensions no longer associative arrays with values that can be passes to the constructor. Constructors should now use WordPress `filters` and `actions` made possible by the new Application Kernel runtime.

```php
/*
|--------------------------------------------------------------------------
| Extensions
|--------------------------------------------------------------------------
|
| The class names of the TypeRocket extensions you wish to enable.
|
*/
'extensions' => [
    '\TypeRocket\Extensions\TypeRocketUI',
    '\TypeRocket\Extensions\PostMessages',
    '\TypeRocket\Extensions\PageBuilder',
    '\TypeRocketPro\Extensions\DevTools', // Pro only
    '\TypeRocketPro\Extensions\Seo', // Pro only
    '\TypeRocketPro\Extensions\ThemeOptions', // Pro only
    '\TypeRocketPro\Extensions\GutenbergRemover' // Pro only
],
```

### New TypeRocket UI Extension

`\TypeRocket\Extensions\PostTypesUI`has been replaced by `\TypeRocket\Extensions\TypeRocketUI`.

### Mailer Service

There is a new `\TypeRocket\Services\MailerService` service listed under the `app.services` config. This service can be removed is you do not want to use the new Pro `Mail` utility.

### Validators

Validators no longer immediately execute when called. You now need to call the `validate()` method on a validator.

```php
$options = [
    'email' => 'required|email',
    'name'  => 'required|min:3'
];

$validator = tr_validator($options, \TypeRocket\Http\Request::new()->getFields())->validate(true);

$errors = $validator->getErrors();
$passes = $validator->getPasses();
```

Also, the `callback` validation rule has changed, you can [read more here](/docs/v5/validator/).