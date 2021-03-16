Title: Upgrade Guide
Description: Upgrade from version 2 of TypeRocket.

---

There are a lot of breaking changes in version 3 since it is a major release. However, there is a lot to be excited about. We want to make sure you can get upgrade if you want to give it a try. 

Because 2.0 to version 3.0 is a major update upgrading is not recommended. It will be best to simply start over if you want the new version. Minor updates like `3.1.0` will be much simpler in the future and not require as many changes.

*WARNING: It is recommended that you backup the older version of your site first then install the new version and manually migrate.*

## Singular Over Plural

Forms, Model and Controllers in version 3.0 all use singular forms. For example forms in 3.0 now look like this:

```php
tr_form('post', 'update', 1);
```

In 2.0 they looked like this.

```php
tr_form('posts', 'update', 1);
```

## New Configuration

The configuration setup is totally new. You will need to make edits in the two new places `config/app.php` and `config/paths.php`.

## New Dot Notation

Instead of bracket syntax switch to the new dot notation. In the past you used `[group][name]` now we use `group.name`.

## New Models

Models are totally new. You will need to change to the new style.

## New Controllers

Controllers are totally new. You will need to change to the new style.

## New Middleware

Middleware is totally new. You will need to change to the new style.

## New Kernel

The Kernel are totally new. You will need to change to the new style. There is no longer an XKernel.