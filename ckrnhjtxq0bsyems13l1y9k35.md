---
title: "Building upon JavaScript with TypeScript"
datePublished: Tue Jun 28 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhjtxq0bsyems13l1y9k35
slug: building-upon-javascript-with-typescript-b2857451f505
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410008043/Tlm-xSDuP.jpeg
tags: javascript, web-development, typescript

---


TypeScript is just a superset of JavaScript. You can run classic JavaScript through the TypeScript compiler and it will come out just fine, just the way it already was. However, once you start using the features of TypeScript that JavaScript doesn’t already have, you start to see its power.

![Photo By [Manik Rathee](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410005742/V8MoipvJl.html)](https://cdn-images-1.medium.com/max/4930/1*YWjOO31sjYKsqIwbbX5VBw.jpeg)*Photo By [Manik Rathee](https://unsplash.com/@manikrathee)*

If you need help setting up a TypeScript development environment, read my previous post titled [Zero to TypeScript Developer using Visual Studio Code on Ubuntu](https://travishorn.com/zero-to-typescript-developer-using-visual-studio-code-on-ubuntu-18ee721136f9#.g9ueg4opk).

Some of the code in this article is modified from the [TypeScript Playground](https://www.typescriptlang.org/play/index.html), which is a useful online “playground” where you can enter TypeScript and see what it looks like when compiled into JavaScript.

Let’s start with a greeter application. When a button is clicked, an alert box will appear greeting whoever we pass to it. We’ll do this without TypeScript first.

%[https://codepen.io/travishorn/pen/oLZEba]

Clicking the button displays “Hello World” without errors. But what happens when we change the argument passed to Greeter()? Instead of a string like it’s expecting, let’s make that an object.

%[https://codepen.io/travishorn/pen/OXpQNJ]

I could easily see this happening in a real-world scenario. A coder forgets what type of argument Greeter() is expecting. There are no JavaScript errors, but when you click the button, an alert pops up that says “Hello, [object Object]”. That’s not what we want at all!

### Adding Types

Fortunately, TypeScript has something called “types”. If we give our argument a type, the TypeScript compiler will know what type of argument to expect.

%[https://codepen.io/travishorn/pen/KMWQMN]

Note that this code will still compile, and it indeed does on CodePen, but in any environment where TypeScript is checked or compiled, you’ll receive the error “Argument of type {…} is not assignable to parameter of type ‘string’”. That’s pretty cool. TypeScript knows that we should only accept a string, and warns us about it if we try to pass anything else. The fixed code looks like…

%[https://codepen.io/travishorn/pen/pbeaRe]

### Using Classes

TypeScript also gives us “classes”. These are logical units of code that can inherit certain properties or have certain properties inherited from themselves.

Our Greeter() function is acting as a class already. Let’s make it an actual class in TypeScript.

%[https://codepen.io/travishorn/pen/LZWQWX]

The class will…

1. contain a property called greeting that is a string

1. execute the constructor function when initialized that sets the greeting. This constructor function accepts a single argument of type string

1. provide a greet() function that returns a greeting

This is all great, but classes don’t really get powerful until you start using inheritance.

### Using Inheritance

We’re going to move away from the greeter example and instead showcase inheritance using examples of real-world animals. We’ll create an Animal class with certain properties, then create a Snake and Horse class that will inherit these properties and add their own.

%[https://codepen.io/travishorn/pen/PzpQOq]

Note: The CodePen embeds might not support viewing the console. Either press F12 and click on “Console” to see the console in this browser window or click “Edit on CodePen” and click on “Console” near the bottom of the new tab that appears.

In this example, we really learn a lot. You can see that Snake and Horse “extend” Animal. This means they have access to all of Animal’s public properties and functions.

By default, an Animal will move 0 meters. We override this with each sub-class: Snake will move 5 by default and Horse will move 45 by default. Each one will print its own method for moving before actually moving, too. Snake will slither and Horse will gallop.

Finally, you can see how the sam variable is instantiated as a Snake and tom is instantiated as a Horse. In this case, we’re also telling TypeScript that tom should be an Animal. Since Horse inherits from the Animal class, it is an Animal, and no errors are thrown.

### Using Generics

Sometimes you’ll want to create a class or function that can accept a variety of types, instead of being locked down to a single one. You could use the “any” type, and it does have a place, but when you use the any type, you lose information about what type was given. In many cases, you want to capture the type and store it in a type variable.

%[https://codepen.io/travishorn/pen/NrpyYb]

In this case, the compiler will set the “T” type variable automatically to match the type of argument we pass in.

### Union Types

In case you need a type to be one of two types, you can use a union type. For example, you might need a type to be either a string or an array of strings.

%[https://codepen.io/travishorn/pen/aZJqaQ]

This is just the tip of the iceberg as far as the functionality TypeScript adds to JavaScript. I recommend checking out the TypeScript Handbook starting with the [Basic Types](https://www.typescriptlang.org/docs/handbook/basic-types.html) page and moving forward.

Some sections you definitely don’t want to skip are…

* Interfaces

* Enums

* Namespaces and Modules

Hopefully you can use this and my previous TypeScript post as a jumping-off point into using TypeScript to write more efficient JavaScript.