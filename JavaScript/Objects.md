# Data Structure: Object, Arrays, OOP: Classes, inheritance, prototype

## Objects

Object is a collection of key value pairs. It is one of the seven data types in JavaScript.

```js
const me = {
  firstName: "Swastik",
  lastName: "Yadav",
  age: 20,
  gender: "Male",
  skills: ["HTML", "CSS", "JavaScript", "React", "NodeJS", "Confident Speaking"]
};
```

That is an object named `me`.

## Arrays

Array is a different form of object. It is a collection of ordered similar values. Think of an object with keys as numbers.

```js
const arrayObj = {
  0: "Zero",
  1: "One",
  2: "Two",
  3: "Three",
  4: "Four"
};

console.log(arrayObj);

// Output: ["Zero", "One", "Two", "Three", "Four"] - Output is an Array.
```

JavaScript automatically converts an object with ordered key value pair (starting from 0) to an Array.

If we need to use a variable as one of the keys, we have to use square brackets.

```js
let animal = "dog";
const obj = {
  [animal]: "barks"
};

console.log(obj.dog);

// Output: "barks"
```

## The "for...in" loop

If we need to loop over the keys of an object, we can for...in loop for that. It loops over all the keys of an object.

Let's loop over the `me` object created above.

```js
for (let key in me) {
  console.log(key, me[key]);
}

// Output: firstName Swastik, lastName Yadav, age 20, gender Male, skills [...]
```

### Copy by Value v/s Reference

Primitive data types are copied by value, means if i manipulate one, other would have no effect. But Objects are copied by reference, means if i manipulate one, other would also get manipulated. This problem is solved by cloning an object. [My blog on this topic.](https://swastikyadav.com/Code%20Snippet/3-ways-to-duplicate-object/). And for cloning nested objects, we use deep cloning algorithms.

## Property Descriptors

Objects have some flags, which are all true by default.

- Writable: false, read only if false
- Enumerable: false, not listed in loops if false
- Configurable: false, can't delete or modify properties if false

Seal an object.

- Object.preventEntension(obj): Frobids the addition of new properties
- Object.seal(obj): Forbids adding / removing of properties
- Object.freeze(obj): Forbids adding / removing / changing of properties

## Property accessors - Getters and Setters

Accessor properties are regular functions that work on getting and setting a value. But look like regular property from outside the object.

```js
const me = {
  firstName: "Swastik",
  lastName: "Yadav",
  get fullName() {
      // getter - the code executed on getting obj.fullName
      return `${this.firstName} ${this.lastName}`;
  }
  set fullName(value) {
      // setter - the code executed on setting obj.fullName = value
      [this.firstName, this.lastName] = value.split(" ");
  }
};

console.log(me.fullName); // Swastik Yadav
me.fullName = "Swastik Bittu"; // sets firstName to Swastik, and lastName to Bittu
console.log(me.fullName); // Swastik Bittu 
```

Notice: We didn't call the function `fullName` ever. It was all handled by getter and setter. Of course this can be done without property accessors, you just have to manually call the function `fullName`.

## THE END