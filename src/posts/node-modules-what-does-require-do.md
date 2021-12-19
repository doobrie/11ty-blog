---
title: Node Modules — What does require() do?
date: 2021-06-27
tags: [node]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628717005/0_Lsduk0TlR0ay2iJm_rgfysj.jpg)

## Introduction

[Node.js](http://nodejs.org) allows code to be written and stored in (preferably) small modules. These modules can then be referenced from other modules to build up larger applications. What exactly is a module though, and how can code be accessed from within a module?

## Exporting functions

Let's take a look at a small piece of code that we can easily turn into a Node.js module. Consider we have a method that allows us to drink tea.

```javascript
function drinkTea() {
  console.log('Mmm, delicious.');
}
```

If this method was placed within a large JavaScript file, we could simply invoke the function `drinkTea()` to enjoy a warm beverage. To create a module however, we simply place this function within its own file and tell Node about any functions we wish callers of the module to access.

Within Node.js, we could therefore create a file called `drink.js` with the following contents:

```javascript
function drinkTea() {
  console.log('Mmm, delicious.');
}

module.exports.drinkTea = drinkTea;
```

You'll notice that this file isn't that different from the original definition of our code. All that we've done to convert this code into a module is to add the `module.exports` statement to the end of the file. This statement tells Node what methods to export from the module.

From a different file, we could then load our tea drinking module and enjoy a cuppa by executing the following code:

```javascript
var drinker = require('./drink');

drinker.drinkTea();
```

## Exporting objects

The example above shows how to export and use a function from a module, but what if we want to export an object? Fortunately, the procedure here is exactly the same. We can create a `Tea` object and export it from a Node module using the following code:

```javascript
var Tea = function(blend) {
  this.blend = blend;
  var that = this;

  return {
    drink: function() {
      console.log('Mmm, ' + that.blend + ', delicious.');
    }
  };
};

module.exports = Tea;
```

We can then invoke this code from a separate `main` module using code such as:

```javascript
var Tea = require('./drink');
var cupOfTea = new Tea('Assam');
cupOfTea.drink();
```

```bash
>node main
Mmm, Assam, delicious
```

The only difference here between the two examples is that in the first example, we exported a named function from our module (`module.exports.drinkTea = drinkTea`). In the second example, we exported the object without any wrapper (`module.exports = Tea`). This allows us to directly create an instance of the module's `Tea` object, i.e. we can call `new Tea...` rather than `new tea.Tea...`

## Credits

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
