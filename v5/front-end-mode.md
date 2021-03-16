Title: Front-end Mode
Description: You can use TypeRocket on the front-end of your site as well.

---

## Enable Front-end Mode

To enable front-end mode for the entire site, set the `front-end` value to `true` in `config/app.php`; it is `false` by default. This will add the TypeRocket required JavaScript and CSS to every page of your theme.

```php
/*
|--------------------------------------------------------------------------
| Front-end
|--------------------------------------------------------------------------
|
| Require TypeRocket on the front-end.
|
*/
'frontend' => true,
```

You can now use TypeRocket on the front-end of your site.

### Single Pages

If you do not want TypeRocket enabled on every page of your site, use the `tr_frontend_enable()` function. Calling `tr_frontend_enable()` within a controller method or at the top of your template file will enable front-end more for that single page.
## Front-end Forms

You can use a [form](/docs/v5/forms/) on the front-end of your site and have it submit to a [custom route](/docs/v5/routes/). To do this you need to create a `Form` and assign it to a URL.

For example, if you have the following route in your `routes/public.php` file.

```php
tr_route()->put()->do('/seats/*', funcion($id) {
    tr_frontend_enable();
    $form = tr_form('seat', $id)->toUrl('/seats/'.$id.'/');
        return tr_view('my.view.file', compact('id', 'form'));
});
```

Then, in your template or view file.

```php
// Print to the view
echo $form->open();
echo $form->text('Number');
echo $form->text('Price');
echo $form->submit('Update Seat');
echo $form->close();
```