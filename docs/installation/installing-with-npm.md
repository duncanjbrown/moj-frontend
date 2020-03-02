# Installing MOJ Frontend with NPM

## Requirements

To use MOJ Frontend with NPM you must:

1. Install the long-term support (LTS) version of [Node.js](https://nodejs.org/en/), which includes NPM. The minimum version of Node required is 4.2.0.

(We recommend using [`nvm`](https://github.com/creationix/nvm) for managing versions of Node.)

2. Create a [package.json file](https://docs.npmjs.com/files/package.json) if you don’t already have one. You can create a default `package.json` file by running `npm init` from the root of your application.

3. If you want to use the MOJ Frontend Nunjucks macros, install Nunjucks - the minimum version required is 3.0.0.

```
npm install nunjucks --save
```

## Installation

To install, run:

```
npm install --save @ministryofjustice/frontend
```

After you have installed MOJ Frontend the `@ministryofjustice/frontend` package will appear in your `node_modules` folder.

## Importing styles

You need to import the MOJ Frontend styles into the main Sass file in your project. You should place the below code before your own Sass rules (or Sass imports) if you want to override MOJ Frontend with your own styles.

1. To import all components, add the below to your Sass file:

```CSS
@import "node_modules/@ministryofjustice/frontend/moj/all";
```

2. To import an individual component (for example a button), add the below to your Sass file:

```CSS
@import "node_modules/@ministryofjustice/frontend/moj/components/button/button";
```

### Optional: Resolving SCSS import paths

If you wish to resolve the above `@import` paths in your build (in order to avoid prefixing paths with `node_modules`), you should add `node_modules` to
your [Sass include paths](https://github.com/sass/node-sass#includepaths) (in Ruby, they should be added to [assets paths](http://guides.rubyonrails.org/asset_pipeline.html#search-paths)).

For example, if your project uses Gulp, you would add the Sass include paths to your Gulp configuration file (for example `gulpfile.js`) with [gulp-sass](https://www.npmjs.com/package/gulp-sass). Below is an example:

```JS
gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass({
      includePaths: 'node_modules'
     }))
    .pipe(gulp.dest('./css'));
});
```

If you compile Sass to CSS in your project, your build tasks will already include something similar to the above task - in that case, you will just need
to include add `includePaths` to it.

After resolving the import paths you can import MOJ Frontend by using:

```CSS
@import "@ministryofjustice/frontend/moj/components/button/button";
```

## Importing assets

In order to import MOJ Frontend images and fonts to your project, you should configure your application to reference or copy the relevant MOJ Frontend assets.

Follow either [Recommended solution](#recommended-solution) or [Alternative solution](#alternative-solution).

### Recommended solution

Make `/node_modules/@ministryofjustice/frontend/moj/assets` available to your project by routing requests for your assets folder there.

For example, if your project uses [express.js](https://expressjs.com/), below is a code sample you could add to your configuration:

```JS
app.use('/assets', express.static(path.join(__dirname, '/node_modules/@ministryofjustice/frontend/moj/assets')))
```
### Alternative solution

Manually copy the images and fonts from `/node_modules/@ministryofjustice/frontend/moj/assets` into a public facing directory in your project. Ideally copying the files to your project should be an automated task or part of your build pipeline to ensure that the MOJ Frontend assets stay up-to-date.

The default paths used for assets are `assets/images` and `assets/fonts`. **If your asset folders follow this structure, you will not need to complete the following steps.**

To use different asset paths, set `$govuk-assets-path`, `$govuk-images-path` and `$govuk-fonts-path` in your project Sass file to point to the relevant directories in your project (this will override the defaults set in `/node_modules/@ministryofjustice/frontend/moj/settings/_assets.scss`). Make sure you do this in Sass before importing `@ministryofjustice/frontend` into your project - see [Importing styles](#importing-styles).

Example 1:

```SCSS
// Include images from /application/assets/images and fonts from /application/assets/fonts
$moj-assets-path: ‘/application/assets’;

@import “@ministryofjustice/frontend/moj/all”;
```

  Example 2:

```SCSS
// Include images from /images/@ministryofjustice/frontend and fonts from /fonts
$moj-images-path: “/images/@ministryofjustice/frontend/moj/”;
$moj-fonts-path: “/fonts/”;

@import “@ministryofjustice/frontend/moj/all”;
```

## Include CSS and JavaScript

Add the CSS and JavaScript code to your HTML template:

```html
<!DOCTYPE html>
  <head>
    <title>Example</title>
    <link rel="stylesheet" href="assets/application.css">
  </head>
  <body>
    <!-- Copy and paste component HTML-->
    <button class="govuk-button">This is a button component</button>
    <script src="assets/application.js"></script>
  </body>
</html>
```

## Using JavaScript

Some of the JavaScript included in MOJ Frontend improves the usability and accessibility of the components.

You should include or import MOJ Frontend JavaScript, and then initialise the appropriate script in your application to ensure that all users can use it successfully.

Note that MOJ Frontend does not initialise any scripts by default; all scripts must be initialised in order for them to work.

### Option 1: Include JavaScript

Include the node_modules/ministryofjustice/moj-frontend/moj/all.js script on your page. You might wish to copy the file into your project or reference it from node_modules.

JavaScript in MOJ Frontend requires HTML to be parsed first by the browser before it is initialised. Because of this, make sure you include the script before the closing `</body>` tag. Including the script elsewhere will stop components from functioning or displaying correctly.

```html
    <script src="path-to-assets/moj-frontend/moj/all.js"></script>
  </body>
</html>
```

MOJ Frontend components with JavaScript behaviour are initialised manually.

To initialise the button menu on a page you can do this:

```js
  new MOJFrontend.ButtonMenu({
    container: $('.button-menu-1'),
    mq: '(min-width: 45em)',
    buttonText: 'Actions'
  });
```

### Option 2: Import JavaScript

If you're using a bundler such as Webpack, use the import syntax to import all components:

```js
import MOJFrontend from 'path/to/all.js';
```

If you're using a bundler such as Webpack, use the import syntax to import a component:

```js
import { ButtonMenu } from 'path/to/all.js';
```

If you're using a bundler such as Browserify, you may need to use the CommonJS require:

```js
const MOJFrontend = require('moj-frontend');
new MOJFrontend.ButtonMenu(...);
```

## Include CSS and JavaScript

Add the CSS and JavaScript code to your HTML template:

```html
<!DOCTYPE html>
  <head>
    <title>Example</title>
    <link rel="stylesheet" href="assets/application.css">
  </head>
  <body>
    <script src="assets/application.js"></script>
  </body>
</html>
```

If your service supports Internet Explorer 8, you will need to [generate and include a separate stylesheet](supporting-internet-explorer-8.md) as well.
