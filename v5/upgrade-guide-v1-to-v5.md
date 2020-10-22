Title: Upgrade Guide v1 to v5
Description: Upgrading TypeRocket v1 to v5

---

**TypeRocket Pro v5** is not fully backward compatible with **TypeRocket v4**. You will need to make a number of updates to get TypeRocket v4 to work. Because Typerocket v5 is such a big update, it is best to create a new TypeRocket v5 project and then migrate your existing v4 sites into TypeRocket v5.

**IMPORTANT NOTICE**: This is not a comprehensive upgrade guide and is still being updated.

## Key Changes

There several critical updates that you need to be aware of when making the update from v1 to v5:

- New component system for the builder and matrix fields.
- New Config and Container System
- New Folder Structure
- The `tr` helper functions have been moved to the project root.
- All `tr_` hooks are now prefixed with `typerocket_`.
- Some classes are now exclusive to Pro: Http and Table.
- Some fields are now exclusive to Pro: Gallery, Location, and Editor.
- Some extensions are now exclusive to Pro: SEO Meta, Hide Post Menu, Gutenberg Remover, Disable Comments, and Theme Options.
- New Pro classes Log, Mail, and Storage.
- New Design.
- New Tabs and Tables APIs.
- Removal of all plugins in favor of extensions system.
- New templating system.
- New CLI system.
- New Core system.
- Upgrade to Webpack.
- And more...

## Removed Helper Functions

All non-prefixed helper functions have been removed like `dd`, `dots_walk`, `class_names`, and `str_contains`. Also, number of helper functions have been removed:

```php
```

### tr_form()

The `tr_form()` function now load the `App/Elements/Form` class instead of `Typerocket/Elements/Form`.

## Pro Extension Publishing

```bash

```