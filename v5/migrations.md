Title: Migrations
Description: Creating and running migrations.

---

## Setting Up Migrations

The TypeRocket migration system requires [having WordPress command support to TypeRocket Galaxy](/docs/v5/galaxy-cli/#section-wp-commands). Once you ensure WordPress commands are available you will see the make migration command `make:migration`.

*Note: Local VM machines will need to make sure the CLI has access to the guest and host machines.*

### Logging and Storing Migrations

TypeRocket migrations are stored in the file system under the `database/migrations` folder. TypeRocket goes about migrations this way because WordPress does not have a standard migrations system.

When a migration is run it creates a database entry in the `wp_options` table with the key `tr_migrations`.

*Note: PRO migrations are stored in the database. This allows for multi-server configurations.*

## Admin UI

You can view the status of your migrations using the `DevTools` extension in your WordPress admin on the Dev page under the migrations tab. If you have [TypeRocket plugins](/docs/v5/plugins-making) their migrations will appear on this page as well.

## Make Migration

To make a migration run, the `make:migration` Galaxy command with a "name" for that migration. There are two types of migrations you can create: SQL and PHP. The default migration type is SQL.

```bash
php galaxy make:migration add_blocks_table
```

Then, in the migration file, add the SQL for your migration. Under the `>>> Up >>>` section add the SQL for migrating up or forward. Under the `>>> Down >>>` section add the SQL for migrating down or backward. Anywhere you add `{!!prefix!!}` the migration system will replace `{!!prefix!!}` with your WordPress table prefix (the default is `wp_`).

```sql
-- Description: For reusable page builder components
-- >>> Up >>>
CREATE TABLE IF NOT EXISTS `{!!prefix!!}blocks` (
`id` int(11) unsigned NOT NULL AUTO_INCREMENT,
`title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
`blocks` longtext COLLATE utf8mb4_unicode_ci DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
-- >>> Down >>>
DROP TABLE IF EXISTS `{!!prefix!!}blocks`;
```

To create a PHP class based migration add `php` after the migration name when using the `make:migration` command.

```bash
php galaxy make:migration add_blocks_table php
```

Will create a PHP migration with this template:

```php
<?php
use TypeRocket\Database\Migration;

return new class($wpdb) extends Migration
{
    public function up()
    {

    }

    public function down()
    {

    }
};
```

## Migrate Up

To migrate "up" and run all migrations that have not yet been run use the command `migrate up`.

```
php galaxy migrate up
```

You can also specify how many migrations you would like to run. For example, to run just one migration you could do the following:

```
php galaxy migrate up 1
```

## Migrate Down

To migrate "down" and drop down one migration that has already been run use the command `migrate down`.

```
php galaxy migrate down
```

You can also specify how many migrations you would like to run. For example, to run just three migrations you could do the following:

```
php galaxy migrate down 3
```

## Migrate Flush

Migrate "down" all migrations that have been run.

```
php galaxy migrate flush
```

## Migrate Reload

Migrate "down" all migrations that have been run and then migrate "up".

```
php galaxy migrate reload
```