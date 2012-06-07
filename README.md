# Ember.hx

>Ember.hx is current under active development.  The examples given below will not yet work!

Ember.hx is a Haxe wrapper allowing you to use the Haxe language to leverage Ember.js - a powerful JavaScript framework that does all of the heavy lifting that you'd normally have to do by hand. There are tasks that are common to every web app; Ember.js does those things for you, so you can focus on building killer features and UI.

These are the features that make Ember.js a joy to use:

1. Bindings
2. Computed properties
3. Auto-updating templates

These are the features that Ember.hx adds to Ember.js, making it even more of a joy to use:

1. Strong typing
2. Code completion
3. Compile time error checking

## Bindings

Use bindings to keep properties between two different objects in sync. You just declare a binding once, and Ember.hx will make sure changes get propagated in either direction.

Here's how you create a binding between two objects:
```haxe
class MyApp extends Ember.Application {
  public static var president:Person;
  public static var country:Country;
  
  public static function main() {
    usPresident = new Person();
    usPresident.name = "Barack Obama";
    
    usa = new Country();
  }
}

class Person extends Ember.Object {
  public var name:String;
}

class Country extends Ember.Object {
  @:binding("MyApp.usPresident.name")
  public var presidentName:String;
}

// Later, after Ember has resolved bindings...
MyApp.usa.presidentName;
// "Barack Obama"
```

Bindings allow you to architect your application using the MVC (Model-View-Controller) pattern, then rest easy knowing that data will always flow correctly from layer to layer.

## Computed Properties

Computed properties allow you to treat a function like a property:

```haxe
class Person extends Ember.Object {
  public var firstName:String;
  public var lastName:String;
  public var fullName(getFullName, null):String;
  
  private function getFullName() {
    return firstName + ' ' + lastName;
  }
}

MyApp.president.fullName;
// "Barack Obama"
```

Treating a function like a property is useful because they can work with bindings, just like any other property.

Many computed properties have dependencies on other properties. For example, in the above example, the `fullName` property depends on `firstName` and `lastName` to determine its value. You can tell Ember.hx about these dependencies like this:

```haxe
class Person extends Ember.Object {
  public var firstName:String;
  public var lastName:String;
  public var fullName(getFullName, null):String;
  
  @:property('firstName', 'lastName')
  private function getFullName() {
    return firstName + ' ' + lastName;
  }
}

MyApp.president.fullName;
// "Barack Obama"
```

Make sure you list these dependencies so Ember.hx knows when to update bindings that connect to a computed property.

## Auto-updating Templates

Ember.hx uses Handlebars, a semantic templating library. To take data from your Haxe application and put it into the DOM, create a `<script>` tag and put it into your HTML, wherever you'd like the value to appear:

``` html
<script type="text/x-handlebars">
  The President of the United States is {{MyApp.president.fullName}}.
</script>
```

Here's the best part: templates are bindings-aware. That means that if you ever change the value of the property that you told us to display, we'll update it for you automatically. And because you've specified dependencies, changes to *those* properties are reflected as well.

Hopefully you can see how all three of these powerful tools work together: start with some primitive properties, then start building up more sophisticated properties and their dependencies using computed properties. Once you've described the data, you only have to say how it gets displayed once, and Ember.hx takes care of the rest. It doesn't matter how the underlying data changes, whether from an XHR request or the user performing an action; your user interface always stays up-to-date. This eliminates entire categories of edge cases that developers struggle with every day.

# Getting Started

For new users, we recommend downloading the [Ember.js Starter Kit](https://github.com/emberjs/starter-kit/downloads), which includes everything you need to get started.

We also recommend that you check out the [annotated Todos example](http://annotated-todos.strobeapp.com/), which shows you the best practices for architecting an MVC-based web application. You can also [browse or fork the code on Github](https://github.com/emberjs/todos).

# Building Ember.js

NOTE: Due to the rename, these instructions may be in flux

1. Run `rake` to build Ember.js. Two builds will be placed in the `dist/` directory.
  * `ember.js` and `ember.min.js` - unminified and minified
    builds of Ember.js

If you are building under Linux, you will need a JavaScript runtime for
minification. You can either install nodejs or `gem install
therubyracer`.

# How to Run Unit Tests

## Setup

1. Install Ruby 1.9.2+. There are many resources on the web can help; one of the best is [rvm](https://rvm.io/).

2. Install Bundler: `gem install bundler`

3. Run `bundle` inside the project root to install the gem dependencies.

## In Your Browser

1. To start the development server, run `rackup`.

2. Then visit: `http://localhost:9292/tests/index.html?package=PACKAGE_NAME`.  Replace `PACKAGE_NAME` with the name of the package you want to run.  For example:

  * [Ember.js Runtime](http://localhost:9292/tests/index.html?package=ember-runtime)
  * [Ember.js Views](http://localhost:9292/tests/index.html?package=ember-views)
  * [Ember.js Handlebars](http://localhost:9292/tests/index.html?package=ember-handlebars)

To run multiple packages, you can separate them with commas. You can run all the tests using the `all` package:

<http://localhost:9292/tests/index.html?package=all>

You can also pass `jquery=VERSION` in the test URL to test different versions of jQuery. Default is 1.7.2.

## From the CLI

1. Install phantomjs from http://phantomjs.org

2. Run `rake test` to run a basic test suite or run `rake test[all]` to
   run a more comprehensive suite.

3. (Mac OS X Only) Run `rake autotest` to automatically re-run tests
   when any files are changed.

# Building API Docs

The Ember.js API Docs provide a detailed collection of methods, classes, and viewable source code.

NOTE: Requires node.js to generate.

See <http://emberjs.com/> for annotated introductory documentation.

## Preview API documenation

* run `rake docs:preview`

* The `docs:preview` task will build the documentation and make it available at <http://localhost:9292/index.html>


## Build API documentation

* run `rake docs:build`

* HTML documentation is built in the `docs` directory
