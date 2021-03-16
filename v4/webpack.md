Title: Webpack with Laravel Mix
Description: Building a theme with Laravel Mix makes managing assets easy.

---

## About Laravel Mix

The [webpack](https://webpack.js.org/) NPM package is a build tool for the front-end assets of your theme. It is also an automation tool to help you manage your development workflow. TypeRocket 4.0 uses webpack to do just that.

Laravel Mix takes advantage of webpack and makes the development process that much better. You can learn more about everything you can do with webpack on [Mix's documentation page](https://laravel.com/docs/5.8/mix).

### Installing Mix

To get started you need to first download the NPM packages required to use mix in the root of your TypeRocket folder.

```bash
npm install
```

## Using

The best place to get started is on the [Mix's documentation page](https://laravel.com/docs/5.8/mix). In TypeRocket your main webpack file `webpack.mix.js` is in the root TypeRocket directory. 


## Development

You are in the development process run `npm run watch` to watch for file changes and update your assets on the fly.

```shell
npm run watch
```

## Production

When your code is ready for production run `npm run prod` to minify and compress your assets.

```bash
npm run prod
```