Title: Front-end Mode
Description: You can use TypeRocket on the front-end of your site as well.

---

## Enable Front-end Mode

To enable front-end mode use the `tr_frontend()` helper method with your themes `functions.php` file. This will add the TypeRocket required JavaScript and CSS to the front end of your theme.

```php
// functions.php
tr_frontend();
```

You can now use TypeRocket on the front-end of your site.

## Front-end Forms

You can use a [form](/docs/v3/forms/) on the front-end of your site and have it submit to a [custom route](/docs/v3/routes/). To do this you need to create a `Form` and assign it to a URL.

For example, if you have the following route in your `routes.php` file.

```php
tr_route()->put('/seats/{id}/', 'update@Seat')
```

Then, in your template or view file wrap the following code in an element with the class `.typerocket-container`.

```php
// A form is best created in a controller
// and passed to a view.
$id = 1;
$form = tr_form('seat', $id);
$form->useURL( 'update', '/seats/'.$id.'/' );

// Print to the view
echo $form->open();
echo $form->text('Number');
echo $form->text('Price');
echo $form->submit('Update Seat');
echo $form->close();
```