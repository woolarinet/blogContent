---
id: 1
img: javascript/js-thumb.png
title: [Core Javascript] 03.this
commentor: sunho
date: 15 July 2021
tag: javascript, core-javascript
description1: What information does this contain when explicit thisBinding is present versus when it is not?
description2: `this` contains information about the calling entity.
descriptions:
In the global scope, this refers to the global object. This is because the entity that creates the global context is the global object. In JavaScript, all variables act as properties of a specific object. Therefore, when declaring a global variable, the JavaScript engine assigns it as a property of the global object.

For example, when you declare and assign the variable a in the global scope and then call a directly, the scope chain searches for a and eventually finds it in the LexicalEnvironment of the global scope, i.e., the global object, and returns its value (i.e., (window.)a).

Thus, in most cases, declaring a variable with var in the global space and directly assigning it to a property of window works the same way. However, there is an exception concerning the delete command, which is intended to prevent unintentional deletions by the user.

When a global variable is declared, the JavaScript engine automatically assigns it as a property of the global object and additionally defines the configurable attribute of that property as false. Therefore, there is a difference between a global variable declared with var and a property of the global object in terms of whether they can be hoisted and whether they are configurable.

Within a method, the caller is the object directly preceding the function name. This object becomes the this context.

When a function is called, the this inside the function is not defined. this is supposed to contain information about the calling entity, but because the function is called without specifying a calling entity and the developer directly runs the code, the calling entity's information is unknown. However, this is considered a design flaw.

What about when a function is called within a method? Similarly, the internal this is not defined and thus refers to the global object. To work around this, developers have used different methods when writing functions within methods. In ES6, to address the issue of this referring to the global object within a function, the arrow function was introduced. The arrow function eliminates the this binding process during the creation of the execution context, allowing it to use the this value from the upper scope directly, making workarounds unnecessary (note that arrow functions cannot be used in an ES5 environment).

JavaScript assigns functions a dual role as constructors (classes), so when a function is called with the new keyword, the function operates as a constructor. In this case, the this within the constructor function refers to the new specific instance being created.

The above explanations cover cases where there is no explicit thisBinding. What happens when thisBinding is specified explicitly?

The call and apply methods explicitly specify this when calling a function or method, while the bind method creates a new function by specifying this and some of the arguments to be passed to the function. Methods that iterate over elements and repeatedly call a callback function may also accept this as a separate argument.
---
