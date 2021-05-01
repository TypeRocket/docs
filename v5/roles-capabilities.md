Title: Roles & Capabilities
Description: Access role and capability features of WordPress through and elegant API.

---

## What Are Roles?

The `Roles` object has several methods for making working with roles and capabilities. The object gives you enhanced APIs for working with [WordPress roles and capabilities](https://wordpress.org/support/article/roles-and-capabilities/).

## List All

Get a list of all roles and their capabilities.

```php
tr_roles()->all();
```

## List Capabilities

Get a list of all capabilities and their asigned roles.

```php
tr_roles()->capabilities();
```

## Get Editable Roles

Fetch a filtered list of user roles that the current user is allowed to edit.

```php
tr_roles()->getEditableRoles();
```

## Add Role

Add a new role and assign capabilities to that role.

```php
tr_roles()->add(
    'typerocket_admin', 
    ['edit_posts' => true, 'delete_posts' => false],
    'TypeRocket Admin'
);
```

## Remove Role

Remove a non-guarded role. Guarded roles include: `administrator`, `editor`, `author`, `subscriber`, `contributor`, and `super_admin` (multi-site).

```php
tr_roles()->remove('typerocket_admin');
```

## Role Exists

Check if role exists. Returns `true` or `false`.

```php
tr_roles()->exists('typerocket_admin');
```

## Get Role

Get the native WordPress `WP_Role` object of a role.

```php
tr_roles()->get('typerocket_admin');
```

## Update Role Capabilities

Add or remove a role's capabilities. To add capabilities:

```php
$capabilities = [
    'read',
    'activate_plugins'
];

tr_roles()->updateRolesCapabilities('typerocket_admin', $capabilities);
```

To remove capabilities:

```php
$capabilities = [
    'read',
    'activate_plugins'
];

$remove = true;

tr_roles()->updateRolesCapabilities('typerocket_admin', $capabilities, $remove);
```

## Custom Post Type Capabilities

When registering a post type with TypeRocket using `tr_post_type()` with [custom capabilities](/docs/v5/post-types/#section-custom-capabilities) you will need to add the custom capabilities to the role you want to have access to it. The `getCustomPostTypeCapabilities()` method allows you to access the list of capabilities you need to add to a role.

```php
// Register Post Type
$books = tr_post_type('Book', 'Books');
$books->customCapabilities();

// Add Custom Capabilities
$cap = tr_roles()->getCustomPostTypeCapabilities('book', 'books');
tr_roles()->updateRolesCapabilities('administrator', $cap);
```

## Custom Taxonomy Capabilities

When registering a taxonomy with TypeRocket using `tr_taxonomy()` with [custom capabilities](/docs/v5/taxonomies/#section-custom-capabilities) you will need to add the custom capabilities to the role you want to have access to it. The `getTaxonomyCapabilities()` method allows you to access the list of capabilities you need to add to a role.

```php
// Register Taxonomy
$pub = tr_taxonomy('Publisher');
$pub->customCapabilities();

// Add Custom Capabilities
$caps = tr_roles()->getTaxonomyCapabilities('publisher', 'publishers');
tr_roles()->updateRolesCapabilities('administrator', $cap);
```