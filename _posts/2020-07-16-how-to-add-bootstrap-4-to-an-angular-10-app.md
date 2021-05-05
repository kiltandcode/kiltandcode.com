---
title: "How to Add Bootstrap 4 to an Angular 10 App"
date: 2020-07-16 21:45:00 +00:00
author: steven
layout: post
image: /assets/img/2020/07/bootstrap-angular.png
icon: angular
tags: angular bootstrap
---

I was trying to add Bootstrap 4 to an Angular 10 app recently and it was more difficult than I expected. We're able to add Bootstrap 4 easily using [NPM](https://www.npmjs.com/) *(Node Package Manager)*, but we still need to install and configure the scripts and dependencies. This can result in a visit to [StackOverflow](https://stackoverflow.com/) which shouldn't be necessary.

{%
    include image.html
    year='2020'
    month='07'
    file='bootstrap-angular.png'
    alt='Angular and Bootstrap'
%}

## Install Bootstrap 4

We can install the latest version of Bootstrap 4 *(at the time of writing this is **4.5.0**)* using NPM. Please adjust the version number if you require a lower or higher version:

```terminal
npm install bootstrap@4.5.0
```

## Install jQuery and Popper

According to the [getting started](https://getbootstrap.com/docs/4.5/getting-started/introduction/) guide, Bootstrap 4 requires the [jQuery](https://jquery.com/) and [Popper](https://popper.js.org/) libraries to be added as dependencies, so we'll also add these using NPM commands:

```terminal
npm install jquery@3.5.1
npm install popper.js@1.14.3
```

## Configure the Styles and Scripts

Finally, in your **angular.json** file all you need to do is reference the CSS and JavaScript files in the styles and scripts sections:

```json
"styles": [
    "node_modules/bootstrap/dist/css/bootstrap.min.css",
    "src/styles.css"
],
"scripts": [
    "node_modules/jquery/dist/jquery.slim.min.js",
    "node_modules/popper.js/dist/umd/popper.min.js",
    "node_modules/bootstrap/dist/js/bootstrap.min.js"
]
```

If you have an issue with Popper not loading correctly, please check you have referenced *node_modules/popper.js/dist/umd/popper.min.js* and **not** *node_modules/popper.js/dist/popper.min.js*.