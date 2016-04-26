---
layout: post
title: Angular 2, Vanilla JS and RequireJS - Part 1
description: "Getting you started with Angular2, JS (not Typescript) and RequireJS"
modified: 2016-04-22
tags: [angular, javascript, requirejs]
categories: [code, tutorials]
custom_js: vendor/jquery-ui.min
custom_css: jquery-ui.min
---
Not ready for Typescript but you really want to try Angular2? So did I. Well, unfortunately, most of the documentation on [angular.io](https://angular.io){:target="_blank"} is for [Typescript](https://www.typescriptlang.org/){:target="blank"}. The hope with this multi-part series is to walk you through the creation of an Angular2 single page app (SPA) which has full User CRUD capabilities taking advantage of all that Angular2 has to offer (services, components, routers, observables, etc). All in vanilla Javascript. 

<!-- more -->

When creating this application I spent a lot of time on Angular's site and I saw a lot of this:
<figure class="twoThirds center">
  <a href="/images/angular_docs_typescript.jpg"><img src="/images/angular_docs_typescript.jpg" alt=""></a>
  <figcaption><a href="https://angular.io/docs/js/latest/guide/architecture.html" title="Angular2 Docs in Typescript Only">What you'll see when you try and view the Anglar2 docs in Javascript</a>.</figcaption>
</figure>

I spent a fair amount of time going through the documentation, stack overflow and other various sites and - __lucky for you__ - figured out how to do just about anything with plain JS that you can do with Typescript.

By the end of this part in the series we will have NodeJS serving up our application in which we will display a component template that is bound to a class variable.

* TOC
{:toc}

## Setting up the Development Environment

### npm
A bit mystifying, [npm](https://npmjs.com) stands for Nodeschool Public Materials. You don't actually _need_ npm but it will facilitate installing NodeJS and setting up and running our localhost server. Fortunately, the [documentation](https://docs.npmjs.com) is very good so we don't need to cover installing and setting it up here. 

### NodeJS
Hopefully, you are at least somewhat familiar with NodeJS. If not I'd suggest looking through the documentation a bit. You won't need any advanced features but we will need to be able to serve static files. So go ahead and [install npm and NodeJS](https://docs.npmjs.com/getting-started/installing-node "Install npm and NodeJS"){:target="_blank"} now. I'll wait right here for you.

We are going to use a [package.json](https://docs.npmjs.com/files/package.json){:target="_blank"} to manage our dependencies. So let's first create a new dir, cd into it and create our file.

Let's create our project directory.
{% highlight bash %}
  $ mkdir angular_tutorial
  $ cd angular_tutorial
  $ npm init
{% endhighlight %}


This will automatically create our package.json file for us. To this we will add the relevent Angular2 packages.

We are going to use the express package to serve up our (for now just static) files. So we need to add this, as well. 

We will create a server.js to house, well... our server code :). Note that we are using express.static and [virtual](http://expressjs.com/en/starter/static-files.html) folder to hide our actual directory structure from visitors to our site. 

#### Project Structure
```
.
├── package.json
├── public
│   ├── app
│   │   └── js
│   │       ├── app.component.js
│   │       └── main.js
│   └── index.html
└── server.js
```
<div id="tabs" class="hidden">
  <ul>
    <li><a href="#package-json">package.json</a></li>
    <li><a href="#server-js">server.js</a></li>
    <li><a href="#index-html">index.html</a></li>
  </ul>

  <div id="package-json">
    {% highlight json %}
    {
      "name": "angular2-tutorial",
      "version": "1.0.0",
      "scripts": {
        "start": "npm run lite",
        "lite": "lite-server"
      },
      "license": "ISC",
      "dependencies": {
        "angular2": "2.0.0-beta.15",
        "es6-shim": "^0.35.0",
        "reflect-metadata": "0.1.2",
        "rxjs": "5.0.0-beta.2",
        "zone.js": "0.6.10"
      },
      "devDependencies": {
        "concurrently": "^2.0.0",
        "lite-server": "^2.2.0"
      }
    }
    {% endhighlight %}
  </div>
  <div id="server-js">
    {% highlight javascript %}
    var express = require('express');
    var path = require('path');
    var app = express();

    /**
     * Serve our static js files - as requested by RequireJS
     */
    app.use(express.static('public'));
    app.use('/libs', express.static(path.join(__dirname, 'node_modules')));
    app.use('/app', express.static(path.join(__dirname, 'public/app')));

    app.listen(3000, function () {
      console.log('Example app listening on port 3000!');
    });
    {% endhighlight %}
  </div>
  <div id="index-html">
    {% highlight html %}
    <html>
      <head>
        <title>Seth Cohen - Angular 2 Crud SPA Example</title>
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">

        <!-- global Angular 2 libraries -->
        <script src="/libs/es6-shim/es6-shim.min.js"></script>
        <script src="/libs/angular2/es6/dev/src/testing/shims_for_IE.js"></script>

        <script src="/libs/angular2/bundles/angular2-polyfills.js"></script>
        <script src="/libs/rxjs/bundles/Rx.umd.js"></script>
        <script src="/libs/angular2/bundles/angular2-all.umd.js"></script>

        <!-- requireJS entry point -->
        <script data-main="/app/js/main.js" src="/libs/requirejs/require.js"></script>
      </head>
      <body>
        <tutorial-app></tutorial-app>
      </body>
    </html>
    {% endhighlight %}
  </div>
</div>
