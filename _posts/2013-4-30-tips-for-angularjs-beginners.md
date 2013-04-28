---
layout: post
comment: 1
title: Tips for AngularJS beginners
categories:
- angularjs
- javascript
---

Some basic angular tips for beginners. These are just things I've notice along the way.

The first tip is to always pass a module name to ng-app.

```html

<!doctype html>
<html ng-app="myBlog">

````

This will tell angular to load your module which you can declare using ```angular.module```

```js
var myBlog = angular.module('myBlog', []);
```

If you use a script loader and are manully bootstrapping your application you can
tell angular about your module by passing the module name as a param to bootstrap.

```js
angular.bootstrap(document, ['myBlog']);
```

This is great because now you'll have a module to create your controller on.


Which leads us to our second tip. Do not use global function as your controllers.
It's pretty rare to find an Angular example that doesn't do this. All these examples
assume you've read the [docs](http://docs.angularjs.org/guide/dev_guide.mvc.understanding_controller)
(which you really should!) and found this very important line burried deep in there.

>
>NOTE: Many of the examples in the documentation show the creation of functions in the
>global scope. This is only for demonstration purposes - in a real application you
>should use the .controller method of your Angular module for your application ...
>

Here is how most examples on the web look

```js
// Bad example.
function myBlogController($scope) {
  $scope.user = 'ericduran';
}
```

Since we now have a module we can follow the docs and use the .controller method to
create our controller.

```js
// Good example.
var myBlog = angular.module('myBlog', []);

myBlog.controller('myBlogController', function($scope) {
  $scope.user = 'ericduran';
});

```

Which leads us to our last tip. Always pass an array to your controller for proper dependency injection when minified.
This would save you some hassle down the road when you're getting ready to deploy your app.

Here is our controller with proper annotation

```js

var myBlog = angular.module('myBlog', []);

myBlog.controller('myBlogController', ['$scope', function ($scope) {
  $scope.user = 'ericduran';
}]);

```

Personally I've gotten use to this format and it doesn't really bother me much. That being said there is a project
called [ngmin](https://github.com/btford/ngmin) by [Brian Ford](https://github.com/btford) (an AgularJS team member)
which can automate this process. It's also available as a [grunt task](https://github.com/btford/grunt-ngmin) which
is great since you can just make this a task before running your minifier.


In short the tips are:

 * Always have a module for your app
 * Don't use global controllers on real apps
 * Make sure to write minifiable code

