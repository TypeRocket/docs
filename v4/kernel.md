Title: Kernel
Description: The Kernel is where middleware is defined and controller is processed.

---

If you submit a `Form` using theme options a `Request` and `Response` (RAR) is created and processed by the `Kernel`. The `Kernel` then tunnels the RAR through correct [middleware](/docs/v4/middleware/) stack. Since theme options use the options resource and used the TypeRocket JSON API the middleware stack is: `VerifyNonce` and `CanManageOptions`.

Here is the `Kernel` configuration out of the box.

```php
<?php

namespace App\Http;

use TypeRocket\Http\Middleware\AuthAdmin;
use TypeRocket\Http\Middleware\AuthRead;
use TypeRocket\Http\Middleware\CanManageCategories;
use TypeRocket\Http\Middleware\CanManageOptions;
use TypeRocket\Http\Middleware\IsUserOrCanEditUsers;
use TypeRocket\Http\Middleware\OwnsCommentOrCanEditComments;
use TypeRocket\Http\Middleware\OwnsPostOrCanEditPosts;

class Kernel extends \TypeRocket\Http\Kernel
{
    public $middleware = [
        'hookGlobal' => [],
        'resourceGlobal' =>
            [
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
    ];
}
```