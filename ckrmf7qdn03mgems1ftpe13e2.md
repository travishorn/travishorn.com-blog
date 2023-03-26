---
title: "Customizing Bootstrap Styles, Step-by-Step"
datePublished: Thu Nov 02 2017 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmf7qdn03mgems1ftpe13e2
slug: customizing-bootstrap-styles-step-by-step-5fdfd60d1116
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410397477/K9gs9jUb0.jpeg
tags: web-design, web-development

---


Bootstrap is an awesome toolkit for front-end development. One of it’s biggest advantages is that when you start to build your app, instead of starting from scratch, you can start building your app using Bootstrap’s styles. You include Bootstrap’s CSS, and use its classes throughout your HTML.

But once you’ve got the prototype for your app, you will probably want to customize these styles to give your app its own branding. Sounds simple enough, but there are many ways to do this. Some of them can get complex.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410395499/em5P5jKpm.jpeg)

### The easy way

The simplest, but least efficient method is to link Bootstrap in your HTML, then link to custom CSS that overrides simple CSS rules. I do not recommend this method.

In `index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>My page</title>

    <link rel="stylesheet" href="bootstrap.css">
    <link rel="stylesheet" href="custom.css">
  </head>
  <body>
    <div class="container">
      <div class="alert alert-success">Congratulations, you've customized Bootstrap styles!</div>
    </div>
  </body>
</html>
```


In `custom.css`:

```
.alert-success {
  background-color: #4CFF00;
}
```


The problems with this method are:

1. Two stylesheets = two HTTP requests = longer loading times

1. The above code changes the successful *alert*’s color, but what about *other* success classes (such as badges or table rows)? You would have to add CSS rules to change the color for all of them.

1. You are including CSS rules to overwrite previous ones. The previous ones are just extra bloat, now.

### The hard way

Another method would be to edit the Bootstrap source and recompile it yourself. I don’t recommend this method either.

1. Install [Node.js](https://nodejs.org/)

1. Install [Ruby](https://www.ruby-lang.org/en/) & [bundler](http://bundler.io/)

1. `npm install` and `bundle install`

1. Download [Boostrap source files](https://getbootstrap.com/docs/4.0/getting-started/download/) and modify them

1. `npm run dist`

1. Link to the newly compiled`bootstrap.css` in your HTML

The main issue I have with this method is that I must install so many heavy development tools that I will never use outside of customizing Bootstrap styles.

Another issue is that when Bootstrap is updated, there is no easy way to merge and resolve conflicts with the custom edits you’ve made.

### The recommended way

This method uses [webpack](https://webpack.js.org/). It is especially advantageous if you’re already using webpack, as there would be little setup. If you’re not already using webpack, the setup process might seem a little convoluted, but it will be beneficial in the long run.

In this guide, I will start from scratch.

1. Install [Node.js](https://nodejs.org/)

1. `cd` into your project’s directory

1. `npm init -y`

1. Add Bootstrap to your project with `npm i -S bootstrap@4.0.0-beta` (The Bootstrap 4 Beta is the latest version as of this writing)

1. Add webpack and the other development dependencies we’ll need with `npm i -D autoprefixer css-loader node-sass postcss-loader precss sass-loader style-loader webpack`

1. In `package.json`, add a build script to run webpack

```
"scripts": {
  // snip
  "build": "webpack"
}
```


7. Tell webpack how to build your app by creating `webpack.config.js`:

```
const precss = require('precss');
const autoprefixer = require('autoprefixer');

module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js',
  },
  module: {
    rules: [{
      test: /\.scss$/,
      use: [
        { loader: 'style-loader' },
        { loader: 'css-loader' },
        {
          loader: 'postcss-loader',
          options: {
            plugins() {
              return [
                precss,
                autoprefixer,
              ];
            },
          },
        },
        { loader: 'sass-loader' },
      ],
    }],
  },
};
```


You don’t have to completely understand this file. Bootstrap styles are built using [Sass](http://sass-lang.com/) and most of this config file just tells webpack how to handle Sass files.

The important thing to understand is that webpack will process `app.js` and spit out `bundle.js` for us.

8. With that being said, create `app.js`:

```
import './style.scss';
```


That’s the only line in this file. In the future, if you want to bundle more modules into your app, whether they be CSS, JavaScript, or something else, you can import them here.

9. `app.js` is importing a Sass file called `style.scss`. Create it like so:

```
@import "custom";
@import "~bootstrap/scss/bootstrap";
```


The first line tells Sass to import a custom stylesheet (which we will create in a minute).

The second line tells it to import Bootstrap styles. The developers of Bootstrap use the `!default` flag on every single Sass variable. So you can override these variables easily in your custom stylesheet.

10. Create `_custom.scss`:

```
$green: #4CFF00;
```


This is where all the fun happens. You can override [any variable you find in Bootstrap’s `_variables.scss`](https://github.com/twbs/bootstrap/blob/v4-dev/scss/_variables.scss).

For example, say I think the default green is too dull and I want to use a more vibrant green throughout my app. I just change that one variable (as shown above) and, though the magic of Sass and Bootstrap, every green element will be changed. This includes green text and borders that might actually be a different color, but calculated from this base green.

11. `npm run build`. This creates `bundle.js`, which now contains all your app’s styles.

To use these styles, just include it in your HTML:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>My page</title>
  </head>
  <body>
    <div class="alert alert-success">Congratulations, you've customized Bootstrap styles!</div>

    <script src="bundle.js"></script>
  </body>
</html>
```


Any time you want to make changes, just edit `_custom.scss` and then run `npm run build`.

### Examples of things to modify

Remember, you can override any variable you find in `[_variables.css`](https://github.com/twbs/bootstrap/blob/v4-dev/scss/_variables.scss).

To customize the styles to your branding, look at editing all of the colors near the top:

```
$blue:    #007bff !default;
$indigo:  #6610f2 !default;
$purple:  #6f42c1 !default;
$pink:    #e83e8c !default;
$red:     #dc3545 !default;
$orange:  #fd7e14 !default;
$yellow:  #ffc107 !default;
$green:   #28a745 !default;
$teal:    #20c997 !default;
$cyan:    #17a2b8 !default;
```


And enabling/disabling any of these optional features:

```
$enable-caret:              true !default;
$enable-rounded:            true !default;
$enable-shadows:            false !default;
$enable-gradients:          false !default;
$enable-transitions:        true !default;
$enable-hover-media-query:  false !default;
$enable-grid-classes:       true !default;
$enable-print-styles:       true !default;
```


Want shadows in your app? Just set `$enable-shadows: true`.

Font faces and sizes can make a big difference in how you app looks:

```
$font-family-sans-serif:      -apple-system, // snip
$font-family-monospace:       "SFMono-Regular", // snip
$font-family-base:            $font-family-sans-serif !default;

$font-size-base:              1rem !default;
$font-size-lg:                1.25rem !default;
$font-size-sm:                .875rem !default;
```


There are a ton of options for customizing forms. Here are just a few:

```
$input-bg:                              $white !default;
$input-disabled-bg:                     $gray-200 !default;

$input-color:                           $gray-700 !default;
$input-border-color:                    $gray-400 !default;
$input-btn-border-width:                $border-width !default;
$input-box-shadow:                      inset 0 1px 1px //snip
$input-border-radius:                   $border-radius !default;
$input-border-radius-lg:                $border-radius-lg !default;
$input-border-radius-sm:                $border-radius-sm !default;
```


Cards can be a huge part of any app. Here’s a sample of the variables that affect cards:

```
$card-spacer-y:                     .75rem !default;
$card-spacer-x:                     1.25rem !default;
$card-border-width:                 $border-width !default;
$card-border-radius:                $border-radius !default;
$card-border-color:                 rgba($black,.125) !default;
$card-inner-border-radius:          calc(#{$card-border-radiu //snip
$card-cap-bg:                       rgba($black, .03) !default;
$card-bg:                           $white !default;
```


The variables in this post are just a sample. There are many others that affect other components, such as badges, modals, navs, progress bars, breadcrumbs. The list goes on and on.

Modify away and `npm run build`.