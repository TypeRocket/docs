Title: JavaScript Hooks
Description: Extend repeaters and matrix fields; or, the REST HTTP response.

---

TypeRocket comes with two callbacks to help you extend the core JavaScript functionality: `TypeRocket.httpCallbacks` and `TypeRocket.repeaterCallbacks`.

## HTTP

The HTTP callback is designed to let you modify and respond to the [REST API](/docs/v4/rest-api/) JSON response.

```javascript
TypeRocket.httpCallbacks.push(function(response) {
   // do something
});
```

### Example: Replacing flash with sweet alert

For example, you can tell the response that it shouldn't use the default flash message but use [sweet alert 2](https://limonte.github.io/sweetalert2/) instead.

```javascript
TypeRocket.httpCallbacks.push(function(response) {
    response.flash = false;

    if (response.valid == true) {
        type = 'success';
        title = 'Success!';
    } else {
        type = 'error';
        title = 'Error!'
    }

    sweetAlert(title, response.message, type);
});
```

If you are using the theme options plugin you can see the core flash message when you save your changes. 

## Repeater and Matrix

The repeater callback is designed to allow you to do something when a repeater or matrix field is added. Although the callback has repeater in its name don't forget it applies to matrix fields as well. 

`$template` is a jQuery object of the field group being added.

```javascript
TypeRocket.repeaterCallbacks.push(function($template) {
    // do something
});
```

### Example: Adding chosen

For example, if you are using [chosen](http://harvesthq.github.io/chosen/) for select fields you can have it run against the template when it is added.

```javascript
TypeRocket.repeaterCallbacks.push(function($template) {
    $template.find('select').chosen();
});
```