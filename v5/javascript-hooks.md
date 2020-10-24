Title: JavaScript Hooks
Description: Extend repeaters and matrix fields; or, the REST HTTP response.

---

TypeRocket comes with two callbacks to help you extend the core JavaScript functionality: `TypeRocket.httpCallbacks` and `TypeRocket.repeaterCallbacks`.

## HTTP

The HTTP callback lets you modify and respond to the [REST API's](https://l.rb.typerocket.test/docs/v5/rest-api/) JSON response.

```javascript
TypeRocket.httpCallbacks.push(function(response) {
   // do something
});
```

### Example: Replacing flash with Sweet Alert

For example, you can tell the response that it shouldn't use the default flash message but use [Sweet Alert ](https://sweetalert.js.org/) instead.

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

    swal(title, response.message, type);
});
```

If you are using the theme options plugin, you can see the core flash message when you save your changes. 

## Repeater and Matrix

The repeater callback is designed to allow you to do something when a repeater or matrix field is added. Although the callback has repeater in its name don't forget it applies to matrix fields as well. 

`$template` is a jQuery object of the field group being added.

```javascript
TypeRocket.repeaterCallbacks.push(function($template) {
    // do something
    $template.find('.my-field').hide();
});
```

## JavaScript Helpers

TypeRocket provides a helper object located at `window.trHelpers`. If you need to access these helpers from the front-end be sure you [have front-end mode enabled](/docs/v5/front-end-mode/).

### Site URL

```js
let site_url = window.trHelpers.site_uri;
```

### Nonce

```js
let tr_nonce = window.trHelpers.nonce;
```