Title: Gulp and Elixir
Description: Building a theme with Gulp and Laravel Elixir makes managing assets easy.

---

## About Gulp and Laravel Elixir

The [Gulp](http://gulpjs.com/) NPM package is a build tool for the front-end assets of your theme. It is also an automation tool to help you manage your development workflow. TypeRocket 3.0 uses gulp to do just that.

Laravel Elixir takes advantage of Gulp and makes the development process that much better. You can learn more about everything you can do with Elixir on [Elixir's documentation page](https://laravel.com/docs/5.3/elixir).

### Installing Gulp and Elixir

To get started you need to first download the NPM packages required to work with Gulp and Laravel Elixir.

```bash
npm install --global gulp-cli
npm install
```

## Using

The best place to get started is on the [Elixir documentation page](https://laravel.com/docs/5.3/elixir). In TypeRocket your main gulp file `gulpfile.js` is in the root TypeRocket directory. 


## Development

You are are in the development process run `gulp watch` to watch for file changes and update your assets on the fly.

```shell
gulp watch
```

## Production

When your code is ready for production run `npm run prod` to minify and compress your assets.

```bash
npm run prod
```