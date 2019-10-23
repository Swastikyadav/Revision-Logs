# Asynchronous JavaScript, Callbacks, Promises, The Event Loop

**Synchronous** - One after another <br />
**Asynchronous** - Simultaneously

While an async task is running, the next line of code does not wait for it to be finished. Next line will get executed before the async task. That is called Asynchronicity. For instance...

Async task usually takes some time, So to simulate that time delay we will use setTimeout function.

```js
setTimeout(() => console.log("Async Task"), 2000);
console.log("Hello World!");

// Output: First "Hello World!" then "Async Task"
```

The problem occurs when our next line code depends on the async task. Because the moment next line code executes, the async task is not finished yet, hence error occurs.

```js
const message = "Not Updated";
function asyncFun() {
  setTimeout(() => {
    message = "Async Task";
  }, 0);
}
asyncFun();
console.log(message);

// Output: "Not Updated"
```

In order to solve this problem we use something called **Callbacks**

## Callbacks

A function which gets accepted as an argument of Higher Order Function (HOF) is a callback function.

Let's use callback function to solve the problem, we discussed above.

```js
var message = "Not Updated";
function asyncFun(callback) {
  setTimeout(() => {
    message = "Async Task";
    callback();
  }, 0);
}
asyncFun(function() {
  console.log(message);
});

// Output: "Async Task"
```

### Error Handling with callbacks

We discussed how to use callback when async task is successfull, but what if async task fails. Here is how we handle errors with callbacks.

```js
function loadScript(src, callback) {
  let script = document.createElement("script");
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}

loadScript("/my/script.js", function(error, script) {
  if (error) {
    console.log(error);
  } else {
    console.log("script loaded successfully");
  }
});

// Output: Error: "Script load error for /my/script.js"
```

Above is a function named loadScript (Script loading is an async task). When async task fails, we call the callback function with error message. Success and failure both cases are handled by the same callback function.

First argument of callback function is always error and rest are for success.

### Callback Hell / Pyramid of Doom

Now say, we need to execute multiple functions one after another. For that we would have to use callback inside callback (Nested Callbacks). That sequence to nested callbacks are called **callback-hell** or pyramid of doom (Because it looks like a horizontal pyramid).

ex...

```js
var message = "Not Updated";
function asyncFun(msg, callback) {
  setTimeout(() => {
    message = msg;
    callback();
  }, 0);
}
asyncFun("callback 1", () => {
  console.log(message);
  asyncFun("callback 2", () => {
    console.log(message);
    asyncFun("callback 3", () => {
      console.log(message);
      asyncFun("callback 4", () => {
        console.log(message);
      });
    });
  });
});

// Output: callback 1, callback 2, callback 3, callback 4
```

This looks pretty ugly, becomes confusing, is hard to manage, and surely it is not the best approach when got to execute multiple functions. That's where promises comes into play. Promises are an another approach for async JS.

## Promises

A promise is a special JavaScript object that links the "producing code" and the "consuming code" together. Promise notifies us when the async task is successful or failure.

### Restaurant Bell

Imagine you ordered food in a restaurant where they have a system that when your food is ready, you will be notified by ringing a bell. Promise is similar to that, it will notify you when your async task is completed or even at failures.

```js
let promise = new Promise(function(resolve, reject) {
  // Executor code
});
```

This is the constructor syntax of promise. The function passed to promise is called executor, which takes two arguments **resolve** and **reject** these two are build in callback functions. When async task is finished one of the above two callback is called, based on success or failure of async task.

```
- resolve(value) - if the job finished successfully, with result value.
- reject(error) - if an error occured, error is error object.
```

Executor runs automatically when promise is created. The returned promise object has some properties:

```
- state: initially "pending", then "fullfilled" when **resolve** is called or "rejected" when **reject** is called.
- result: initially "undefiened", then "value" when **resolve(value)** is called or "error" when **reject(error)** is called.
```

When promise's state is either fulfilled or rejected, the promise is said to be settled.

```js
let promise = new Promise(function(resolve, reject) {
  // resolved promise
  setTimeout(() => resolve("done"), 1000);
});
```

```js
let promise = new Promise(function(resolve, reject) {
  // rejected promise
  setTimeout(() => reject(new Error("Oops")), 1000);
});
```

### then, catch, finally

We can subscribe to promise with `then, catch, finally`, so that promise notifies us about the status of async task.

#### then

```js
promise.then(
    result => { /* Handle a success result */ },
    err => { /* Handle a failure, err */ }
);
```

**then** takes two callback functions, first one handles success while second one handles failure.  - If we only care about the case of success, we can skip the second (err) function. 
 - If we only care about the case of error, we replace first (result) function with `null`.


```js
promise.then(
    null,
    err => { /* Handle a failure, err */ }
);
```

#### catch

Catch is just a syntactic sugar for the previous code example of `then`, where we replaced first function with `null`. The code below and the previous one are exactly same.

```js
promise.catch(
    err => { /* Handle a failure, err */ }
);
```

#### finally

The function in finally always runs when the promise is settled (either resolved or rejected). It can be used to do something that has to be done no matter what is the state of promise. Like Stop CSS loader when promise is settled.

```js
promise.finally(
    () => stop css loader
);
```