Title: Mail - Mailgun
Description: Custom Mailgun email driver setup.

---

## Getting Started

!!

You can add mailgun to your site by setting the driver in your `config/mail.php` file. Or, you can set the driver using the `typerocket_mailer_service_driver` and `typerocket_mail_driver_mailgun_options` filters.

Options include:

- `region` - Two options `us` or `eu`.
- `api_key` - You can use the API key from the Mailgun "Settings" > "API Keys" section, or a domain specific API key under "Sending" > (select your domain) > "Domain Settings" > "Sending API keys".
- `domain` - Your custom sending domain, eg. mg.example.com, not the Mailgun API domain.
- `from_override` - A `bool` value. When set it will use the config's address information as the "From:" information used instead of the code level "From:" information. Setting this value to `true` will send all emails from the same address.
- `from_address` - The email address used by the `from_override` and as the default "From:" email address.
- `from_name` - The name used by the `from_override` and as the default "From:" name for the email address.
- `track_clicks` - Two options `yes` and `no`.
- `track_opens` - Two options `yes` and `no`.
- `tags` - A list of tags to mark email sends with in the Mailgun dashboard.

```php
add_filter('typerocket_mail_driver_mailgun_options', function($driver) {
    return [
        'region' => 'us', // eu is another option
        'api_key' => 'key-somerandomchars',
        'domain' => 'mg.example.com',
        'from_override' => false,
        'from_address' => 'kevin@example.com',
        'from_name' => 'Kevin from TypeRocket',
        'track_clicks' => 'no',
        'track_opens' => 'no',
        'tags' => ['WordPress'],
    ];
});

add_filter('typerocket_mailer_service_driver', function($driver) {
    return new \TypeRocketPro\Utility\Mailers\MailgunMailDriver();
});
```

*Note: The `api_key` can be either your main API private key or a domain specific API key.*

### Mailgun UI

As an example, you might want to make a user interface for your Mailgun driver config.

```php
// MailGun
$form->group('mailgun')->fieldset('Mailgun', 'Email deliverability', [
    $form->toggle('Use Mailgun'),
    $form->section(
        $form->text('Mailgun API Key')->setName('api_key')->setAttribute('autocomplete', 'new-password')
            ->setHelp('Your Mailgun API key. Only valid for use with the API.'),
        $form->select('Mailgun Region')->setName('region')->setOptions(['U.S./North America' => 'us', 'Europe' => 'eu'])
            ->setHelp('Choose a region - U.S./North America or Europe - from which to send email, and to store your customer data. Please note that your sending domain must be set up in whichever region you choose.'),
        $form->text('Mailgun Domain')->setName('domain')->setHelp('Your Mailgun Domain Name.'),
        $form->text('Mailgun From Address')->setName('from_address')->setHelp('It is recommended that the @mydomain portion matches your Mailgun sending domain.'),
        $form->text('Mailgun From Name')->setName('from_name')->setHelp('The "User Name" part of the sender information ("Excited User <user@samples.mailgun.org>").'),
        $form->toggle('Override "From" Details')->setName('from_override')->setText('Send all mail from the above address.'),
        $form->toggle('Mailgun Click Tracking')->setName('track_clicks')->setText('If enabled, Mailgun will track links.'),
        $form->toggle('Mailgun Open Tracking')->setName('track_opens')->setText('If enabled, HTML messages will include an open tracking beacon.'),
    )->when('use_mailgun')
]);
```

Then, you can save the Mailgun data to your `wp_options` table with the key `mailgun` to automatically set the config for the Mailgun driver.

```php
add_filter('typerocket_mailer_service_driver', function($driver) {
    $mg = get_option('mailgun');
    if(is_array($mg) && !empty($mg['use_mailgun'])) {
        $driver = new \TypeRocketPro\Utility\Mailers\MailgunMailDriver();
    }
    return $driver;
});
```