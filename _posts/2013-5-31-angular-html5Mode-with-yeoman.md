---
layout: post
comment: 1
title: AngularJS HTML5 Mode with Yeoman
categories:
- angularjs
- javascript
---

HTML5 Mode in AngularJS is awesome, you should use it. The one issue people run into
is that you end up having a lot of 404 when a user tries to refresh at an unknow path.

This is mainly because your AngularJS routes aren't actual html pages. An example would
be if you have a route in your angular app to /create-order. This url works fine if you
link to it from inside your app but if a user tries to go directly to that page the server
will return a 404.

This is because AngularJS HTML5 mode uses the History API to
push a new url to your broswer. Sadly this does require some extra work on the server side
to have those url return the correct content.

Here are some quick ways to fix this:

If you're using Yeoman you can easily add a new middleware to the Livereload server.

First you need to install connect-modrewrite middleware (I tried a couple of different
connect redirect middleware, this was the simplest one).

```
 npm install connect-modrewrite
```

Then you need to tell your livereload server about the new middleware.
Lets make some additions to our GruntFile.js
Ex

```js
  // Yeoman's GruntFile.js

  // You can place this outside your module.exports function.
  var modRewrite = require('connect-modrewrite');

  ...

  // Now lets modifies our server config and add our modRewrite middleware.
  livereload: {
    options: {
      middleware: function (connect) {
        return [
          modRewrite([
            '!\\.html|\\.js|\\.css|\\.png$ /index.html [L]'
          ]),
          lrSnippet,
          mountFolder(connect, '.tmp'),
          mountFolder(connect, yeomanConfig.app)
        ];
      }
    }
  }
```

This tells your server to send the index.html file for any request that isn't a .html, .js, .css, or .png.
You can add other exceptions if require (svg, etc..).

If you're running your angular app on an apache server you can easily add this rule in an .htaccess file.

```
# Apache .htaccess

# angularjs pushstate (history) support:
# See http://www.josscrowcroft.com/2012/code/htaccess-for-html5-history-pushstate-url-routing/
<ifModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} !index
    RewriteRule (.*) index.html [L]
</ifModule>
```
