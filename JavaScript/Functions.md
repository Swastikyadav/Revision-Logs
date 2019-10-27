# Functions: Scopes, Recursion, Spread / Rest ..., Higher order function, Closures, Function methods

## Scopes

Scope in simple term means "Where to look for the value of a variable ?". For ex a variable decleared inside a function can't be accessed outside of that function. Scopes are mainly of two types: Global scope and Local Scope.

Global Scope: A variable defined globally, which is accessible to everywhere in the code.
Locla Scope: A variable defined inside a block, which can not be accessed outside of that block.

There are 3 kind of variables: `const`, `let`, and `var`.

Const and let creates a block everywhere, but var creates a block only inside a function.

Lexical Scope: A lexical scope in Javascript means that a variable defined outside a function can be accessible inside another function defined after the variable declaration. But the opposite is not true, the variables defined inside a function will not be accessible outside that function.

## Recursion

Recursion is a programming term that means calling a function from itself. Recursive functions can be used to solve tasks in elegant ways.

```js
// Function to find the power.
function pow(x, n) {
  return n === 1 ? x : x * pow(n - 1);
}

pow(2, 3); // 2*2*2 = 8
```

And that was a very simple example of recursive function. Any recursive function can be rewritten into an iterative one. Because ultimately recursion is also loop. We can decide over regular loop or recrsion depending on the situation.

## Rest parameters... and Spread operators.

Rest parameters: A function can accept multiple arguments, and say we need to treat all those arguments as if they are in an array. So, we use rest parametes...

For instance.

```js
// Sum of all arguments.
function sumAll(...args) {
  let sum = args.reduce((acc, cv) => {
    acc += cv;
    return acc;
  });
  console.log(sum);
}
sumAll(5, 4, 3, 7); // 19 - We used all arguments as if they were inside an array.
```

Spread Operators: It does the exact opposite of `rest parametes...`. It spreads out all the elements from an array. For ex, Math.max() takes multiple numbers, how do we pass an array to it? The answer is spread operators.

```js
let arr = [5, 4, 8, 2, 6, 55];
let arr1 = [7, 20, 101, 63, 12];

Math.max(...arr, ...arr1);
// 101
```

## Higher Order function (HOF)

HOF: Functions which accepts another function definition as a parameter or returns a function definition is a Higher Order Function.

And the functions being accepted as parameter are callbacks. All string and array methods are HOF. Because they require a callback to work.

```js
let arr = [1, 2, 3];
arr.map(function(e) {
  return e * 2;
});
// [2, 4, 6], Map is a higher order function, it accepts a function definition as parameter.
```

## Closures

When a function is returned from another function, the returned function carries a bag (object) of environment of parent function (that bag / object contains all variables and methods defined inside the parent funciton), hence returned function can access those environment even from outside of parent function.

That object / bag is called closure.

```js
function name(name) {
  let age = 20;
  return function() {
    console.log(`Name: ${name}, Age: ${age}`);
  };
}

let inner = name("Swastik Yadav");
inner(); // Name: Swastik Yadav, Age: 20
```

## Funtion Methods (call, bind, apply)

### Losing `this`: When passing object methods as callbacks, for instance to `setTimeOut()`, the value of `this` is lost. ex...

```js
let user = {
    firstName: "Swastik",
    lastName: "Yadav",
    sayHi: function() {
        console.log(`Hello ${this.firstName} ${this.lastName}`);
    }
}

setTimeout(user.sayHi, 1000); // Hello Undefined Undefined
```

You See! The value of this is lost and output of `this` is `Undefined`. To solve this problem we can bind / fix the value of `this` so that it remains same no matter what.

```js
let user = {
    firstName: "Swastik",
    lastName: "Yadav",
    sayHi: function() {
        console.log(`Hello ${this.firstName} ${this.lastName}`);
    }
}

let sayHi = user.sayHi.bind(user); // `this` is fixed / bound to user.

setTimeout(sayHi, 1000); // Hello Swastik Yadav
```

apply() and call() methods are also pretty similar. 

- apply(): Parameter is an array.
- call(): Limited Parameter.

## THE END