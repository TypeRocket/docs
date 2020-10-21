Title: Icons
Description: Icons are used in the admin menu to help signify the menu item visually.

---

## About Icons

TypeRocket icons are mainly used in the admin menu for custom admin pages and custom post types. To see all of the available icons enable the `dev` TypeRocket plugin and browse the list of icons on the "Dev" admin page it creates.

## Override Icons 

If you want to override the default icons and use your own create a new `MyIcons` class in the `app` directory as `app/MyIcons.php`.

For example, if you wanted to use the [Font Awesome](http://fontawesome.io/) fonts for `MyIcons` you would need to enqueue the font styles and font files from Font Awesome. Then, assign the correct values to the `MyIcons` class.

```php
<?php
namespace App;

use TypeRocket\Elements\Icons;

class MyIcons extends Icons
{
    public $fontFamily = 'FontAwesome';
    public $fontSize = '17px/1';
    public $icons = [
        'tags' => '\f02c',
        'book' => '\f02d',
        'bookmark' => '\f02e',
        'rocket' => '\f135',
        // ...
    ];
}
```

*Note: The `$icons` property should be an array of the icons name and the CSS content property inclosed in single quotes.*

Finally, in the `config/app.php` file switch the icon setting out for your new class.

```php
'icons' => \App\MyIcons::class,
```

## Icon List

- `cog`
- `flag`
- `headphones`
- `qrcode`
- `crosshairs`
- `plane`
- `calendar`
- `magnet`
- `key`
- `cogs`
- `link`
- `flask`
- `paperclip`
- `magic`
- `money`
- `dashboard`
- `umbrella`
- `lightbulb`
- `stethoscope`
- `suitcase`
- `coffee`
- `cutlery`
- `building`
- `ambulance`
- `beer`
- `desktop`
- `laptop`
- `tablet`
- `mobile`
- `gamepad`
- `keyboard`
- `terminal`
- `code`
- `location-arrow`
- `code-fork`
- `puzzle-piece`
- `microphone`
- `shield`
- `fire-extinguisher`
- `anchor`
- `bullseye`
- `ticket`
- `euro`
- `gbp`
- `dollar`
- `rupee`
- `yen`
- `ruble`
- `won`
- `sun`
- `moon`
- `pagelines`
- `wheelchair`
- `space-shuttle`
- `slack`
- `bank`
- `graduation`
- `paw`
- `spoon`
- `cube`
- `cubes`
- `car`
- `tree`
- `life-saver`
- `rebel`
- `send`
- `bomb`
- `soccer`
- `binoculars`
- `plug`
- `newspaper-o`
- `wifi`
- `calculator`
- `eyedropper`
- `paint-brush`
- `area-chart`
- `pie-chart`
- `line-chart`
- `bicycle`
- `bus`
- `diamond`
- `street-view`
- `heartbeat`
- `venus`
- `mars`
- `battery`
- `map-pin`
- `map-signs`
- `map`
- `credit-card-alt`
- `usb`
- `shopping-bag`
- `shopping-basket`
- `envira`
- `wheelchair-alt`
- `home`
- `office`
- `newspaper`
- `pencil`
- `quill`
- `droplet`
- `paint-format`
- `image`
- `images`
- `camera`
- `music`
- `video-camera`
- `dice`
- `bullhorn`
- `podcast`
- `feed`
- `mic`
- `book`
- `books`
- `stack`
- `folder`
- `price-tag`
- `price-tags`
- `barcode`
- `cart`
- `credit-card`
- `envelop`
- `location`
- `location-fill`
- `compass`
- `clock`
- `alarm`
- `bell`
- `stopwatch`
- `printer`
- `tv`
- `drawer`
- `floppy-disk`
- `drive`
- `database`
- `bubble`
- `bubbles`
- `user`
- `users`
- `user-tie`
- `hour-glass`
- `color-wheel`
- `disk`
- `open-disk`
- `search`
- `key2`
- `key3`
- `lock`
- `unlocked`
- `wrench`
- `equalizer`
- `gear`
- `hammer`
- `bug`
- `trophy`
- `gift`
- `glass`
- `glass2`
- `mug`
- `spoon-knife`
- `rocket`
- `meter`
- `gavel`
- `fire`
- `lab`
- `trash`
- `briefcase`
- `truck`
- `road`
- `accessibility`
- `power`
- `clipboard`
- `node-tree`
- `menu`
- `cloud`
- `cloud-download`
- `cloud-upload`
- `sphere`
- `earth`
- `attachment`
- `eye`
- `eye-blocked`
- `bookmark`
- `bookmarks`
- `contrast`
- `brightness-contrast`
- `star-empty`
- `star-half`
- `star-full`
- `heart`
- `heart-broken`
- `man`
- `woman`
- `man-woman`
- `spell-check`
- `volume`
- `loop`
- `infinite`
- `shuffle`
- `checkbox`
- `make-group`
- `scissors`
- `filter`
- `omega`
- `sigma`
- `table`
- `amazon`
- `google`
- `facebook`
- `twitter`
- `steam`
- `github`
- `tux`
- `apple`
- `android`
- `windows`