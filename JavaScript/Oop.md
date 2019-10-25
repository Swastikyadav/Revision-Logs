# OOP: Object Oriented JavaScript, Prototype, Inheritance, Classes

In JavaScript objects have a special hidden property that is either null or reference other object, that object is called "prototype".

That prototype contains all the object methods.

## Prototypal nature of object.

If a property is not found in an object, JS goes down into protypal chain (prototype) to look for that property. We can also add our custom properties and methods in prototype of an object.

## __proto__ (dunder proto)

__proto__ is just getter and setter for prototype. It is not same as prototype. __proto__ is a very old way of accessing or modifing prototype, but it still exists for some historical reasons. There are better ways to work with prototype, for instance ...

- Object.getPrototypeOf(obj): returns the prototype of object.
- Object.setPrototypeOf(obj, proto): sets teh prototype of obj to proto.

These should be used instead of __proto__.

**for...in loop also iterates over inherited custom properties in prototype.** That's because those properties have enumerable flag as true. So, to solve this issue, we can use `Object.hasOwnProperty(prop)` with `for...in` loop.

## Different ways of creating an object.