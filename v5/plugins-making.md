Title: Making WordPress Plugins
Description: The Galaxy CLI helps developers build custom TypeRocket powered WorDPress plugins.

---

*Pro Only: This is a Pro only extension feature.*

## Make A TypeRocket Powered WordPress Plugin

To create a standard WordPress plugin that is powered by TypeRocket use the galaxy CLI command.

```bash
php galaxy make:plugin {phpNamespace} "{plugin name}"
```

This command will download a boilerplate plugin into your `wp-content/plugins` folder. For example, run the command:

```bash
php galaxy make:plugin MyPluginName "My Plugin"
```

Once run you will be asked to if you want to generate a Galaxy CLI clone in the root of your WordPress installation.

```
> php galaxy make:plugin MyPluginName "My Plugin"
Clone GALAXY CLI for My Plugin to project root?
```

If you input `y` you will see the following:

```bash
GALAXY CLI clone: ~/wordpress/galaxy_my_plugin
Plugin "My Plugin" with app/MyPluginTypeRocketPlugin.php created at ~/wordpress/wp-content/plugins/my-plugin
```

You can use the new `~/wordpress/galaxy_my_plugin` CLI script to run the Galaxy CLI system using the context of your plugin. Finally, your TypeRocket plugin code should be added to `~/wordpress/wp-content/plugins/my-plugin/app/MyPluginTypeRocketPlugin.php`.

Now, if you want to run a galaxy command within the plugin's context use the `galaxy_my_plugin` script. For example, to make a migration for the plugin run:

```bash
php galaxy_my_plugin make:migration add_my_custom_table
```

## Core Plugin Class

The core plugin class includes a lot of features to help you get started. It includes:

1. An `app/View.php` class for loading views inside the plugin folder `resources/views`.
2. A `webpack.mx.js` file, `resources/js`, `resources/sass`, and `public` for your front-end code.
3. A `composer.json` if it is needed.
4. A `database/migrations` folder for any plugin specific migrations.
5. An `app` folder for your custom application PHP classes.
6. An `uninstall` script, but you can use the `app/MyPluginTypeRocketPlugin.php` for the uninstallation process.

The `app/MyPluginTypeRocketPlugin.php` class is where all of your custom code and be added. This class includes a few methods out of the gate:

1. `init()` - Place all of your custom plugin code like WordPress hooks here. By default, TypeRocket includes the hooks needed to load your plugin's front-end assets and settings page.
2. `routes()` - Place your custom routes here instead of the main TypeRocket `public.php` routes file.
3. `policies()` - If you are using policies in your plugin add them here not the `AuthSerivce` class (`AuthSerivce` can override your policies).
4. `activate()` - Place any activation code you need here. By default, any migrations will be run when the plugin is activated because of `migrateUp()` and permalink flushed because of `tr_update_site_state()`.
5. `deactivate()` - Place any deactivation code here. By default, permalinks will be flushed because of `tr_update_site_state()`.
6. `uninstall()` - Place nay uninstallation code here. By default, any migrations will be unwound because of `migrateDown()`. You may want to add your own `tr_update_site_state()` here as well.

### View Plugin Migrations

Any migrations loaded into a plugin can also be seen from the `DevTool` page under migrations.

## Plugin Config

TypeRocket plugins do not include a config system. Also, you should not make any edits to your main `app` or `config` regarding your new plugin. Plugins should remain separate from your main TypeRocket app.