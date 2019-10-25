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
}
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
}

console.log(arrayObj);

// Output: ["Zero", "One", "Two", "Three", "Four"] - Output is an Array.
```

JavaScript automatically converts an object with ordered key value pair (starting from 0) to an Array.

If we need to use a variable as one of the keys, we have to use square brackets.

```js
let animal = "dog";
const obj = {
    [animal]: "barks"
}

console.log(obj.dog);

// Output: "barks"
```

### The "for...in" loop

If we need to loop over the keys of an object, we can for...in loop for that. It loops over all the keys of an object.

Let's loop over the `me` object created above.

```js
for(let key in me) {
    console.log(key, me[key]);
}

// Output: firstName Swastik, lastName Yadav, age 20, gender Male, skills [...]
```

### Copy by Value v/s Reference

Primitive data types are copied by value, means if i manipulate one, other would have no effect. But Objects are copied by reference, means if i manipulate one, other would also get manipulated. This problem is solved by cloning an object. [My blog on this topic.](https://swastikyadav.com/Code%20Snippet/3-ways-to-duplicate-object/). And for cloning nested objects, we use deep cloning algorithms.

