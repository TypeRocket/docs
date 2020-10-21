Title: Installation
Description: How to install and configure TypeRocket. PHP 5.3+ and WordPress 4.0+.

---

## Environment

The TypeRocket framework has a few system requirements.

- PHP >= 5.3
- WordPress >= 4.0
- PDO PHP Extension
- Friendly URLs

## Installation

To install TypeRocket 2.0 you need to [download it from GitHub](https://github.com/TypeRocket/typerocket/releases/tag/v2.0.13) into your theme. Make sure you name the TypeRocket folder as `typerocket`.

*Note: You can rename the folder later on.*

### Initialization

At the top of your themes `functions.php` file require `typerocket/init.php`. This will initialize TypeRocket and complete the installation.

```php
<?php // functions.php

require ('typerocket/init.php');
```

## Configuration

Before you can get started you need to configure TypeRocket. All configuration will be stored in a configuration file called `config.php` that you need to create.

The configuration file needs to be created within the `typerocket` folder. To help you, there is a  `config.sample.php` file that you can duplicate and rename to `config.php`.

## Enable API: Flushing Rewrite Rules

To enable the REST API needed for TypeRocket to function properly you need to [flush the WordPress permalinks](https://l.rb.typerocket.test/flushing-permalinks-in-wordpress/). This is because the REST API registers rewrite rules. You can flush the permalinks by going to in the admin:

1. Settings
2. Permalinks
3. Scroll to the bottom of the permalinks page
4. Click "Save Changes"

![Flush permalinks](https://l.rb.typerocket.test/wp-content/uploads/2015/08/save-permalinks.png)

*Note: Any time you register a post type or change a post type ID you need to do the same.*