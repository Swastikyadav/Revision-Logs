# OOP: Object Oriented JavaScript, Prototype, Inheritance, Classes

In JavaScript objects have a special hidden property that is either null or reference other object, that object is called "prototype".

That prototype contains all the object methods.

## Prototypal nature of object.

If a property is not found in an object, JS goes down into protypal chain (prototype) to look for that property. We can also add our custom properties and methods in prototype of an object.

## **proto** (dunder proto)

**proto** is just getter and setter for prototype. It is not same as prototype. **proto** is a very old way of accessing or modifing prototype, but it still exists for some historical reasons. There are better ways to work with prototype, for instance ...

- Object.getPrototypeOf(obj): returns the prototype of object.
- Object.setPrototypeOf(obj, proto): sets teh prototype of obj to proto.

These should be used instead of **proto**.

**for...in loop also iterates over inherited custom properties in prototype.** That's because those properties have enumerable flag as true. So, to solve this issue, we can use `Object.hasOwnProperty(prop)` with `for...in` loop.

## Different ways of creating an object.

### Factory way of creating object.

```js
function user(name, age, gender) {
  var obj = {};
  obj.name = name;
  obj.age = age;
  obj.gender = gender;
  obj.greet = function() {
    return `Hello ${name}`;
  };
  return obj;
}

const swastik = user("Swastik", 20, "Male");
// { name: "Swastik", age: 20, gender: "Male", greet: function... }
const john = user("John", 30, "Male");
// { name: "John", age: 30, gender: "Male", greet: function... }
```

But there is a problem with this approach. Everytime the user function is called, greet method is also created. It also takes some extra memory space. Greet method is same for all users, hence it should be created only once, and used by all users.

Let's solve it, with another way of creating objects.

### Prototypal way of creating object.

There are 3 steps to create an object with protypal way.

- Use Object.create()
- Use `this`
- return object

Object.create(): Creates a new object, takes an object as parameter and replace the prototype of newly created object by that parameter.

```js
let userMethod = {
  greet: function() {
    return `Hello ${this.name}`;
  }
};

function user(name, age, gender) {
  var obj = Object.create(userMethod);
  obj.name = name;
  obj.age = age;
  obj.gender = gender;
  return obj;
}

const swastik = user("Swastik", 20, "Male");
// { name: "Swastik", age: 20, gender: "Male" }
swastik.greet();
// Hello Swastik
```

Here greet method is created only once, but is being used by every newly created object. That's because Object.create(userMethod) replaces the prototype of that object with greet method.

This is a great way of creating an object. But we can simplify it even further. Remember the 3 steps of creating object in prototypal way. What if we automate it, and we need not to do it manually. Well, let's do it.

Before procceding further, we need to understand few things.

#### Contructor Function

A function which creates an object is called constructor function. It is a general convention capitalize the name of constructor function. And constructor function should only be called with `new` keyword.

#### `new` keyword

new does 3 things at max:

- Creates new object. (That obj is names `this`) - Implicitly
- Replaces new object's prototype with F.prototype.
- Return newly created object. - Implicitly

You see! All 3 steps is beign done by `new` alone. Task automated. `new` is only used with constructor function.

```js
function User(name, age, gender) {
  this.name = name;
  this.age = age;
  this.gender = gender;
}

User.prototype = {
  greet: function() {
    return `Hello ${this.name}`;
  }
};

let swastik = new User("Swastik", 20, "Male");
console.log(swastik);

// { name: "Swasitk", age: 20, gender: "Male" }

swastik.greet();
// Hello Swastik
```

Much more efficient than the previous one.

#### `this` keyword

`this` is a pointer, that points to different objects depending on the situation. This changes at run time. There are 4 rules to figure out where `this` is pointing.

- From inside a regular fn() `this` points to window object.
- From inside a method (fn() inside object) `this` always points to the respective object.
- bind(), call(), apply(). From inside fn methods value of `this` is specified.
- new, From inside a constructor fn value of `this` is created and returned implicitly.

### Pseudo classical way of creating objects.

Using classes to do everything that we did above.

Class:

- Class is just a syntactic sugar. (It is same a constructor function)
- Class can only accept methods. (As of now)

```js
class User {
    constructor(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    greet() {
        return `Hello ${this.name}`
    }
}

const swastik = new User("Swastik", 20, "Male");
swastik.greet();
// Hello Swastik
```

We can directly write method inside class. Not need to manually put in prototype, that's all has been automated.

What's happening here is same as in 'prototypal way of creating object', but just with automation and a better syntax.

To extend a class: `class Child extends Parent`:

  - That means `Child.prototype.__proto__` will be `Parent.prototype`, so methods are inherited.

When overriding a constructor:

  - We must call parent constructor as `super()` in Child constructor before using `this`.

When overriding another method:

  - We can use super.method() in a Child method to call Parent method.

```js
class Human {
    constructor(name) {
        this.name = name;
    }

    eat() {
        console.log(`Human ${this.name} eats`);
    }

    sleep() {
        console.log(`Human ${this.name} sleeps`);
    }
}

// Extending Human class
class Male extends Human {
    constructor(name, gender = "male") {
        super(name); // Super executes the constructor of parent (Human) class.
        this.gender = gender;
    }

    sleep() {
      super.sleep(); //
      console.log(`Human ${this.name} sleeps at least 6 hours`);
    }
}

const swastik = new Male("Swastik");
swastik.sleep(); // Human Swastik sleeps, Human Swastik sleeps at least 6 hours
```

This is how we extends a class and override constructor and other methods in class.

### Static properties and methods

We can also assign methods to class function itself, not to its prototype. And such methods are called static methods. `this` inside such methods points to class itself.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  static staticMethod() {
    cosnole.log(this);
  }

  getThis() {
    console.log(this);
  }
}

const user1 = new User("Swastik");
user1.staticMethod(); // Err, no such method exists. - Because staticMethod() is a method of class itself, not of its prototype.
User.staticMethod(); // User fn()

user1.getThis(); // user1 { name: "Swastik" }, getThis() is a method of prototype of class.
``

## THE END