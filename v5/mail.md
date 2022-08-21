Title: Mail
Description: Custom email system.

---

## Getting Started

!!

To WordPress send email WordPress provides the `wp_mail()` function. With new service `\TypeRocket\Services\MailerService` loaded in your `app.servies` config file you can extend the WordPress email system with additional mail drivers.

## Configuration

To configure your mail service to a specific mail driver see the `app/mail.php` config file. By default, TypeRocket uses its `wp` mail driver to emulate the `wp_mail()` function. There are three mail drivers included with TypeRocket:

- `wp`: The classic WordPress mail driver.
- `mailgun`: [the mailgun driver](/docs/v5/mail-mailgun/) for sending email with [mailgun.com](https://www.mailgun.com/) via their HTTP API.
- `log`: A driver that logs all email sent by WordPress to your the [TypeRocket logs](/docs/v5/log/).

You can set your default mail driver in the `mail.default` config setting. The default driver will then be used by the `wp_mail()` function and the TypeRocket `\TypeRocketPro\Utility\Mail` class.

```php
wp_mail( $to, $subject, $message);

\TypeRocketPro\Utility\Mail::new()
    ->to($to)->subject($subject)
    ->message($message)
    ->send();
```

## Mail Class

The fluent TypeRocket Mail class enables you to send mail in a more intuitive way. To start working with the `\TypeRocketPro\Utility\Mail` class create an instance of it.

```php
$mail = \TypeRocketPro\Utility\Mail::new();
```

## To

```php
$mail->to('hello@example.com');

echo $mail->to();
// Outputs: hello@example.com

$mail->to(['hello@example.com', 'world@example.com']);

echo $mail->to();
// Outputs: hello@example.com, world@example.com
```

## From

```php
$mail->from('hello@example.com');

echo $mail->from();
// Outputs: hello@example.com

$mail->from(['hello@example.com', 'world@example.com']);

echo $mail->from();
// Outputs: hello@example.com, world@example.com
```

## Reply To

```php
$mail->replyTo('hello@example.com');

echo $mail->replyTo();
// Outputs: hello@example.com
```

## Subject

```php
$mail->subject('Open this email');

echo $mail->subject();
// Outputs: Open this email
```

## Message

By default, the mail is sent as HTML.

```php
$mail->message('<p>Your message!<p>');

echo $mail->message();
// Outputs: <p>Your message!<p>
```

To send a message as plain text:

```php
$mail->asText();
// or,
$mail->header('Content-Type', 'text/plain; charset=UTF-8');
```

You can also send [views](/docs/v5/views/) as your email message.

```php
<?php // resources/views/mail/simple.php ?>
<p>Your view message!<p>
```

```php
// You will need a view
$mail->view('mail.simple');

echo $mail->message();
// Outputs: <p>Your view message!<p>
```

## Headers

```php
$mail->header('From', 'hello@example.com');
$mail->headers([
    'From' => 'hello@example.com',
    'Reply-To' => 'world@example.com',
]);
```

## Attachments

```php
$attachments = [
    WP_CONTENT_DIR . '/uploads/2020/11/image.png',
    WP_CONTENT_DIR . '/uploads/2020/11/image2.png',
];

$mail->attachments($attachments);
```

## Driver

If you do not want to use the default mail driver your can specify a different one.

```php
$mail->driver(new TypeRocketPro\Utility\Mailers\MailgunMailDriver);
```

## Send

Once you are ready to send use the `send()` method.

```php
$mail->send();
```