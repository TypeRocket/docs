Title: Log
Description: Custom log system.

---

*Pro Only: This is a Pro only extension feature.*

## Getting Started

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