Title: Migrations
Description: Creating and running migrations.

---

## Setting Up Migrations

The TypeRocket migration system requires [enabling WordPress command support to TypeRocket Galaxy](/docs/v4/galaxy-cli/#section-wp-commands). Once you enable WordPress commands you will see a new make command `make:migration`

![typerocket-migration](https://l.rb.typerocket.test/wp-content/uploads/2019/04/typerocket-migration.png)

### Logging and Storing Migrations

TypeRocket migrations are logged and stored in the file system under the `sql` folder. TypeRocket goes about migrations this way because WordPress does not have a standard migrations system. Keep in mind that if you have multiple servers architecture only one of them should run migrations because TypeRocket uses a file system to log migrations.

## Make Migration

To make a migration run the `make:migration` Galaxy command with a "name" for that migration.  

```
php galaxy make:migration add_blocks_table
```

Then, in the migration file add the SQL for your migration. Under the `>>> Up >>>` section add the SQL for migrating up or forward. Under the `>>> Down >>>` section add the SQL for migrating down or backward.

Anywhere you add `{!!prefix!!}` the migration system will replace `{!!prefix!!}` with your WordPpess table prefix (the default is `wp_`).

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
DROP TABLE `{!!prefix!!}blocks`;
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

To migrate "down" and run all migrations that have already been run use the command `migrate down`.


```
php galaxy migrate down
```

You can also specify how many migrations you would like to run. For example, to run just three migrations you could do the following:

```
php galaxy migrate down 3
```

