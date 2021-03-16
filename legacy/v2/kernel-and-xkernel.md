Title: Kernel and XKernel
Description: The Kernel is where middleware is defined and controller is processed.

---

## Kernel

If you submit a `Form` using theme options a `Request` and `Response` (RAR) is created and processed by the `Kernel`. The `Kernel` then tunnels the RAR through correct [middleware](/docs/middleware/) stack. Since theme options use the "options resource" the middleware stack is `AuthRead`, `ValidateCsrf` and `CanManageOptions`.

Here is the `Kernel` configuration out of the box.

```php
<?php
namespace TypeRocket\Http;

class Kernel
{

    protected $middleware = array(
        'hookGlobal' =>
            array('AuthRead'),
        'restGlobal' =>
            array(
                'AuthRead',
                'ValidateCsrf'
            ),
        'noResource' =>
            array('AuthAdmin'),
        'users' =>
            array('IsUserOrCanEditUsers'),
        'posts' =>
            array('OwnsPostOrCanEditPosts'),
        'pages' =>
            array('OwnsPostOrCanEditPosts'),
        'comments' =>
            array('OwnsCommentOrCanEditComments'),
        'options' =>
            array('CanManageOptions')
    );

    /* ...... */

}

```

## XKernel

If you want to create your own middleware configuration you can extend the `Kernel` with a class called `XKernel` under `typerocket/app/Http/XKernel.php`. This class will be used in place of the `Kernel` class and tunnel the RAR through the middleware you define.

```php
<?php // typerocket/app/Http/XKernel.php
namespace TypeRocket\Http;

class XKernel extends Kernel
{

    protected $middleware = array(
        'hookGlobal' =>
            array('AuthRead'),
        'restGlobal' =>
            array(
                'AuthRead',
                'ValidateCsrf'
            ),
        'noResource' =>
            array('AuthAdmin'),
        'users' =>
            array('IsUserOrCanEditUsers'),
        'posts' =>
            array('OwnsPostOrCanEditPosts'),
        'pages' =>
            array('OwnsPostOrCanEditPosts'),
        'comments' =>
            array('OwnsCommentOrCanEditComments'),
        'options' =>
            array('CanManageOptions')
    );

}

```