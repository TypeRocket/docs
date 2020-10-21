Title: Post Types: Secured
Description: Secure custom fields using models, controllers, middleware and the CLI.

---

In a previous tutorial, we created a [stacked "team" post type](/docs/v3/post-types-making/) with custom fields. As you experienced, TypeRocket saves any and all fields. However, this level of flexibility is not always appropriate for security reasons.

## Security

If you are an advanced developer you may only want certain fields to be fillable. For the "team" post type, whose ID we set to `tr_team`, the only fillable fields needed are `photo`, `job_title` and `post_content `. Making these fields the only fillable ones will prevent all other fields from being saved. This will make the new post type more secure and protect us from certain request attacks.

## Model and Controller

To limit saving data to only the `photo`, `job_title` and `post_content` fields a `Person` model and `PersonController` needs to be created. Use the TypeRocket CLI `galaxy` to create both the model and controller files, forgoing the manual process of creating the files.

From the TypeRocket folder run:

```shell
php galaxy make:model -c post Person tr_team
```

This creates a model class at `app/Models/Person.php`. And, because we used the `-c` flag, a controller class is created at `app/Controllers/PersonController.php` outputting:

```shell
Model created: Person as Post
Controller created: PersonController as Post
```

## Set Fillable Fields

By setting the fillable property of the `Person` model we will tell WordPress to only save the specified fields.

```php
<?php // In /app/Models/Person.php
namespace App\Models;

use \TypeRocket\Models\WPPost;

class Person extends WPPost
{
    protected $postType = 'tr_team';

    protected $fillable = [
        'photo',
        'post_content',
        'job_title'
    ];
}
```

Once the fillable fields are set, in the WrdPress backend if debug mode is enabled, you will see a pencil icon next to the field label of each fillable field.

![typerocket fillable icons](https://l.rb.typerocket.test/wp-content/uploads/2015/07/typerocket-fillable.png)

Now, only `photo`, `job_title` and `post_content` will be saved. However, because the resource controller and model has been specified only an administrator can manage the new fields. If you need to customize who can update the fields configure your HTTP Kernel and Middleware.

## Kernel and Middleware

Middleware can be used for all kinds of fun things. For the `PersonController` we want the `person` resource to authenticate as a post type.

By default any resource without a named middleware group will default to the `noResource` group. To customize what middleware group `PersonController` routes through, add a group named `person`. Add the `person` middleware group to the bottom of the `Kernel` middleware property.


```php
<?php // /app/Http/Kernel.php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' =>
            [ AuthRead::class ],
        'resourceGlobal' =>
            [
                AuthRead::class,
                Middleware\VerifyNonce::class
            ],
        'noResource' =>
            [ AuthAdmin::class ],
        'user' =>
            [ IsUserOrCanEditUsers::class ],
        'post' =>
            [ OwnsPostOrCanEditPosts::class ],
        'page' =>
            [ OwnsPostOrCanEditPosts::class ],
        'comment' =>
            [ OwnsCommentOrCanEditComments::class ],
        'option' =>
            [ CanManageOptions::class ],
        'category' =>
            [ CanManageCategories::class ],
        'tag' =>
            [ CanManageCategories::class ]
        'person' => // added here
            [ OwnsPostOrCanEditPosts::class ]
    ];
}

```

## Format Fields

Back at our `Person` model we can also format the `photo` when it is being saved. This allows us to filter and sanitize data before it is place in the database. 

*If you need to keep the original state of data in the database then you can skip this step.*

For the `photo` field formatting it as an integer is best, since we reference it by the attachment ID. To do this define the format property and set an array key of the field name and the value to a callback.

For the `photo` field we will use the [php function intval](http://php.net/manual/en/function.intval.php) as the callback. 

```php
<?php // In /app/Models/Person.php
namespace App\Models;

use \TypeRocket\Models\WPPost;

class Person extends WPPost
{
    protected $postType = 'tr_team';

    protected $fillable = [
        'photo',
        'post_content',
        'job_title',
    ];

    protected $format = [
        'photo' => 'intval' // set the photo field to an integer
    ];
}
```