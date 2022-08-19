Title: Log
Description: Custom log system.

---

## Getting Started

! **Pro Only**: This is a Pro only extension feature.

To configure the log system to a specific log driver see the `app/logging.php` config file. By default, TypeRocket uses its `stack` log driver. There are five log drivers included with TypeRocket:

- `file`: Logs daily files to the TypeRocket `storage/logs` folder.
- `slack`: Logs to Slack using [webhooks](https://api.slack.com/messaging/webhooks).
- `mail`: Send a log to an email address.
- `null`: Stops logging.
- `stack`: Logs to a list of other drivers.

## Log Levels

```php
\TypeRocketPro\Utility\Log::emergency('My log');
\TypeRocketPro\Utility\Log::alert('My log');
\TypeRocketPro\Utility\Log::critical('My log');
\TypeRocketPro\Utility\Log::error('My log');
\TypeRocketPro\Utility\Log::warning('My log');
\TypeRocketPro\Utility\Log::notice('My log');
\TypeRocketPro\Utility\Log::info('My log');
\TypeRocketPro\Utility\Log::debug('My log');
```

## Switch Driver

If you do not want to use the default driver you can specify one.

```php
\TypeRocketPro\Utility\Log::driver('file')->emergency('My log');
```

## Custom Stacks

You can log to a custom stack as well.

```php
\TypeRocketPro\Utility\Log::stack(['file', 'mail'], 'emergency', 'My log');
```

## File Logger

You can customize the location the file logger saves log files by defining the `wp-config.php` constant `TYPEROCKET_LOG_FILE_FOLDER`. Or, you can define the `typerocket_log_file` filter hook; but keep in mind your hook must be added before `\TypeRocketPro\Utility\Log` is called:

```php
add_filter('typerocket_log_file', function($file, $folder, $message, $options) {
    return $file;
}, 10, 4)
```

### Options

You can define how log files should be broken down by defining the `wp-config.php` constant `TYPEROCKET_LOG_FILE_OPTIONS`. **Main** breakdown options include:

1. `daily` - Breaks log files into daily dated file names like `typerocket-2022-08-18.log`.
2. `single` - Makes logs a single file `typerocket.log`. Using this setting can result in a large log file.

Further, you can define a type level break down of how log files are stored by using the following format `{main}:{type}`. **Type** breakdown options include:

1. `joined` - Makes logs a single file based on the `typerocket-2022-08-18.log`.
2. `split` - Breaks log files into type specific file names like `typerocket-debug.log`, `typerocket-info.log`, `typerocket-error.log`, and such.

### Examples

For example, a setting of `daily:joined`, this is the default setting, will result in log files named by date: `typerocket-2022-08-18.log`. Whereas `daily:split` will result in log files named by date and type: `typerocket-2022-08-18-debug.log`. Finally, `single:split` will result in log files named by type only: `typerocket-debug.log`.