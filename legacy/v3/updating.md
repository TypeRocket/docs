Title: Updating
Description: Updating TypeRocket 3.0 core and assets.

---

Updating Typerocket core and assets is now very easy, with the introduction of `composer` and `npm`. When updating core update the assets as well.

*Note: If you are on version 3 of TypeRocket see [version 4 the upgrade guide](/docs/v4/upgrade-guide/).*

## Core

The `composer.json` file is used to manage the version of TypeRocket you have installed. Take a look at the initial setup. Under the require section locate `typerocket/core`. There you will see that when you run `composer update` you will receive any patch updates because you are requesting `3.0.*`.

```json
"require": {
    "php": ">=5.5.9",
    "typerocket/core": "3.0.*"
}
```

## Assets

In some cases, there may be updates to the TypeRocket assets. Assets include JavaScript, fonts, CSS, and images. To manage the updates to these files use [NPM](https://www.npmjs.com/). You can find the [TypeRocket NPM package](https://www.npmjs.com/package/typerocket-assets) on the NPM website. 

1. To update the assets first run `npm update`. 
2. Next, uncomment the TypeRocket code at the bottom of `gulpfile.js`. 
3. Finally, run `npm run prod`. This will recompile all of your project assets and the TypeRocket core assets.

```js
/*
 |--------------------------------------------------------------------------
 | Update TypeRocket Assets
 |--------------------------------------------------------------------------
 |
 | Uncomment this section if you want to update the TypeRocket assets each
 | time you compile your resources. Run `npm update typerocket-assets`
 | to check for new versions.
 |
 */

var typerocket = require('typerocket-assets');
l.rb.typerocket.testpileTypeRocketAssets( './wordpress/assets' );
```