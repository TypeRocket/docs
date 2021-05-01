Title: Post Types: Securing
Description: Secure custom fields using models, controllers, middleware and the CLI.

---

## Securing Post Type Models

In a previous tutorial, we created a [stacked "team" post type](/docs/v5/post-types-making/) with custom fields. As you experienced, TypeRocket saves any and all fields. However, this level of flexibility is not always appropriate for security reasons.

## Security

If you are an advanced developer, you may only want specific fields to be fillable. For the "team" post type, the only fillable fields needed are `photo`, `job_title` and `post_content`. Making these fields the only fillable ones will prevent all other fields from being saved. This will make the new post type more secure and protect us from certain request attacks.

## Model and Controller

To limit saving data to only the `photo`, `job_title` and `post_content` fields a `Person` model and `PersonController` needs to be created. Use the TypeRocket CLI `galaxy` to create both the model and controller files, forgoing the manual process of creating the files.

From the TypeRocket folder run:

```shell
php galaxy make:model -c post Person
```

This creates a model class at `app/Models/Person.php`. And, because we used the `-c` flag, a controller class is created at `app/Controllers/PersonController.php` outputting:

```shell
Model created: Person as Post
Controller created: PersonController as Post
```

## Apply Model & Controller

Next, apply the model and controller to the team post type. This step is not required but makes life easier later.

```php
$team->setHandler(\App\Controllers\PersonController::class);
$team->setModelClass(\App\Models\Person::class);
```

## Set Fillable Fields

By setting the fillable property of the `Person` model, we will tell WordPress only to save the specified fields.

```php
<?php // In /app/Models/Person.php
namespace App\Models;

use \TypeRocket\Models\WPPost;

class Person extends WPPost
{
    public const POST_TYPE = 'person';

    protected $fillable = [
        'photo',
        'post_content',
        'job_title'
    ];
}
```

Once the fillable fields are set, in the WordPress backend, if debug mode is enabled, you will see a pencil icon next to the field label of each fillable field.

![typerocket fillable icons](https://typerocket.com/wp-content/uploads/2015/07/typerocket-fillable.png)

Now, only `photo`, `job_title` and `post_content` will be saved.

## Format Fields

Back at our `Person` model we can also format the `photo` when it is being saved. This allows us to filter and sanitize data before it is placed in the database. 

*If you need to keep the original state of data in the database then you can skip this step.*

For the `photo` field formatting it as an integer is best, since we reference it by the attachment ID. To do this define the format property and set an array key of the field name and the value to a callback.

For the `photo` field we will use the [php function intval](http://php.net/manual/en/function.intval.php) as the callback. 

```php
<?php // In /app/Models/Person.php
namespace App\Models;

use \TypeRocket\Models\WPPost;

class Person extends WPPost
{
    public const POST_TYPE = 'person';

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