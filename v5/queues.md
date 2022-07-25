Title: Queues & Jobs
Description: Add delayed jobs to be run at a later date.

---

## About Queues

The TypeRocket queues system allows code execution to happen at a later time. For example, some long-running tasks like sending email with `wp_mail()` can slow down an HTTP request's response time while it waits for PHP to send the email. By using TypeRocket queues and jobs, you can dispatch the sending of the email for a later time, thus significantly increasing the response time of an otherwise slow request.

The job and queue system is build on top of the [WooCommerce Action Scheduler](https://actionscheduler.org/) because it is widely used and does not create collisions like other composer packages.

## Getting Started

To start using the queues and jobs system, you first need to add the `TypeRocket\Services\JobQueueRunner` class to your config's `app.services` settings. Once added to your services list you can begin adding jobs to the queue system.

```php
'services' => [
    /*
     * TypeRocket Service Providers...
     */
    '\TypeRocket\Services\ErrorService',
    '\TypeRocket\Services\MailerService',
    
    /*
     * Application Service Providers...
     */
    '\App\Services\AuthService',
    'TypeRocket\Services\JobQueueRunner', // <- Added here
],
```

## Jobs

Jobs are PHP classes that are designed to run at a later time once they are dispatched. When a job is dispatched it enters the queue system and the queue system will run jobs added to it using [WP CRON](https://developer.wordpress.org/plugins/cron/). To create a job class sue the galaxy CLI command:

```bash
php galaxy make:job MyFirstJob
```

This command will create a `App\Jobs\MyFirstJob` job class in your configured `app/Jobs` folder. Further, the command will also add the job to your `queue.jobs` registry. If your job class is not registered it can not be added to the queue. So, if you create a Job class manually you will need to register it manually as well.

```php
<?php
namespace App\Jobs;

use TypeRocket\Utility\Jobs\Interfaces\AllowOneInSchedule;
use TypeRocket\Utility\Jobs\Interfaces\WithoutOverlapping;
use TypeRocket\Utility\Jobs\Job;

class MyFirstJob extends Job
{
    public function handle()
    {
        // Do stuff when job is called
        $payload = $this->payload;
    }
}
```

And, added to the `queue.jobs` config registry.

```php
'jobs' => [
    \App\Jobs\MyFirstJob::class,
]
```


### Queue A Job

Now that your Job has been created you can dispatch it to the queue from your plugins or theme after TypeRocket is loaded. Again, you need to add your job to the `queue.jobs` registry.

```php
App\Jobs\MyFirstJob::dispatch();
```

### Payloads

To pass information along to your job you can send an array of data. The data will be JSON encoded.

```php
App\Jobs\MyFirstJob::dispatch([
    'email' => 'kevin@example.com',
    'name' => 'Kevin'
]);
```



