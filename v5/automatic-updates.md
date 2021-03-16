Title: Automatic Updates
Description: Add automatic updates to your plugin or theme.

---

## Getting Started

*Pro Only: This is a Pro only extension feature.*

TypeRocket provides an API for adding your own automatic updates system. This API can be used if TypeRocket is installed as an MU plugin or if TypeRocket is installed as root.

The updaters check for a new version every 12 hours.

## Plugins

The slug must match the name of your plugin.

```php
new \TypeRocketPro\Updates\PluginUpdater([
    'slug' => 'my-typerocket-plugin',
    'api_url' => 'https://example.com/plugins/my-typerocket-plugin/'
]);
```

The required endpoint format is JSON.

```json
{
  "name": "My TypeRocket Plugin",
  "slug": "my-typerocket-plugin",
  "version": "1.0.0", // Plugin version
  "requires": "5.3", // WP version
  "tested": "5.3", // WP version
  "requires_php": "7.0",
  "upgrade_notice": "Plugin update is available.",
  "author": "<a class='https://robojuice.com'>Robojuice</a>",
  "homepage": "https://example.com",
  "contributors": [
    {
      "display_name": "Kevin Dees",
      "profile": "https://profiles.wordpress.org/kevindees",
      "avatar": "https://secure.gravatar.com/avatar/d7b2627f714dab61e2eea164c3500d35?s=96&d=mm&r=g"
    }
  ],
  "banners": {
    "high": "https://example.com/banner.png",
    "low": null
  },
  "sections": {
    "description": "<p>My plugin</p>",
    "installation": "<p>Install things this way...</p>",
    "changelog": "<h4>1.0.0</h4><ul><li>Sart project.</li></ul>"
  },
  "download_url": "https://example.com/downloads/latest-version.zip",
  "last_updated": "2020-01-01 14:59:53"
}
```

## Themes

The slug must match the name of your theme.

```php
new \TypeRocketPro\Updates\ThemeUpdater([
    'slug' => 'my-typerocket-theme',
    'api_url' => 'https://example.com/themes/my-typerocket-theme/'
]);
```

The required endpoint format is JSON.

```json
{
  "name": "My TypeRocket Theme",
  "slug": "my-typerocket-theme",
  "version": "1.0.0", // Theme version
  "requires": "5.3", // WP version
  "tested": "5.3", // WP version
  "requires_php": "7.0",
  "author": "<a class='https://robojuice.com'>Robojuice</a>",
  "homepage": "https://example.com",
  "contributors": [
    {
      "display_name": "Kevin Dees",
      "profile": "https://profiles.wordpress.org/kevindees",
      "avatar": "https://secure.gravatar.com/avatar/d7b2627f714dab61e2eea164c3500d35?s=96&d=mm&r=g"
    }
  ],
  "download_url": "https://example.com/downloads/latest-version.zip",
  "last_updated": "2020-01-01 14:59:53"
}
```
