Title: Form Honeypot
Description: Form honeypot protection.

---

## Getting Started

A form honeypot can help reduce the amount of spam your receive. You can add a honeypot to your forms by using `tr_honeypot_fields()` function inside your front-end forms.

```php
$form = tr_from([])->toRoute('form');
echo $form->open();
echo $form->honeypot();
echo $form->text('Name');
echo $form->close('Save');
```

Or, with HTML forms.

```html
<form action="form-endpoint">
<?php echo tr_honeypot_fields(); ?>
</form>
```

Next, you need to check the honeypot to see if a bot touched it. Do this by adding the `\TypeRocket\Http\Middleware\CheckSpamHoneypot` [middleware](/docs/v5/middleware/) to the middleware group your form is using.

```php
class Kernel extends HttpKernel
{
    protected $middleware = [
        'hooks' =>
            [],
        'http' => [ 
            \App\Http\Middleware\VerifyNonce::class,
            \TypeRocket\Http\Middleware\CheckSpamHoneypot::class,
        ],
        ...
    ];
}
```

Make sure your form request passes through the Kernel by using a [custom route](/docs/v5/routes/) so the middleware is used. Or, you might want to add the middleware directly to a route instead of using a middleware group.

```php
tr_route()
    ->post()
    ->name('form')
    ->middleware([\TypeRocket\Http\Middleware\CheckSpamHoneypot::class])
    ->on('form-endpoint', function() { return 'success'; });
```

