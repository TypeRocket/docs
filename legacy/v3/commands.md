Title: Commands
Description: Build your own galaxy CLI commands.

---

## Making a Command

To make your own galaxy CLI command use the galaxy command `make:command`. Galaxy commands use the [Symfony Console Package](http://symfony.com/doc/current/console.html). Lets make a command named `app:test`.

```shell
php galaxy make:command Test app:test
```

This will output,

```shell
Command created: Test
Configure Command Test: Add your command to config/galaxy.php
```

You can now find your new command at `app/Commands/Test.php`. We will take a look at it next, but first add the command to the `galaxy.php` configuration file.

```php
'commands' => [
    \App\Commands\Test::class
]
```

Now run `php galaxy list` and check for:

```shell
app
  app:test         Short description here
```

Now run the command:

```shell
 php galaxy app:test
```

This will output,

```shell
Executed!
```

You are now ready to start building your command.

## Coding Commands

Here is the `Test` command class we just created. If you want to make your commands without the galaxy CLI this can serve as a working template.

```php
<?php
namespace App\Commands;

use \TypeRocket\Console\Command;

class Test extends Command
{

    protected $command = [
        'app:test',
        'Short description here',
        'Longer help text goes here.',
    ];

    protected function config()
    {
        // If you want to accept arguments
        // $this->addArgument('arg', self::REQUIRED, 'Description');
    }

    public function exec()
    {
        // When command executes
        $this->success('Execute!');
    }
}
``` 


