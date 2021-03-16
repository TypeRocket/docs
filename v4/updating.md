Title: Updating
Description: Updating TypeRocket 3.0 core and assets.

---

Updating Typerocket core and assets is now very easy, with the introduction of `composer` and `npm`. When updating core update the assets as well.

## Core

The `composer.json` file is used to manage the version of TypeRocket you have installed. Take a look at the initial setup. Under the require section locate `typerocket/core`. There you will see that when you run `composer update` you will receive any patch updates because you are requesting `4.0.*`.

```json
"require": {
    "php": ">=7",
    "typerocket/core": "4.0.*"
}
```

## Assets

In some cases, there may be updates to the TypeRocket assets. Assets include JavaScript, fonts, CSS, and images. To manage the updates to these files use [NPM](https://www.npmjs.com/). 

To update the assets you will need to recompile the core JS and CSS files. To recompile install the NPM modules and run the NPM `prod` command.

From the main `typerocket` folder run:

```
npm install
npm run prod
```