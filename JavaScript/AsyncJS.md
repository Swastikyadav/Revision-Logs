# Asynchronous JavaScript, Callbacks, Promises, The Event Loop

**Synchronous** - One after another <br />
**Asynchronous** - Simultaneously

While an async task is running, the next line of code does not wait for it to be finished. Next line will get executed before the async task. That is called Asynchronicity. For instance...

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
	}, 0)
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
	}, 0)
}
asyncFun(function() {console.log(message)});

// Output: "Async Task"
```

### Error Handling

We discussed how to use callback when async task is successfull, but what if async task fails. Here is how we handle errors in with callbacks.

```js
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}

loadScript('/my/script.js', function(error, script) {
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
function asyncFun(msg ,callback) {
    setTimeout(() => {
        message = msg;
        callback();
    }, 0)
}
asyncFun("callback 1" ,() => {
    console.log(message);
    asyncFun("callback 2", () => {
        console.log(message);
        asyncFun("callback 3", () => {
            console.log(message);
            asyncFun("callback 4", () => {
                console.log(message);
            })
        })
    })
});

// Output: callback 1, callback 2, callback 3, callback 4
```

This looks pretty ugly, becomes confusing, is hard to manage, and surely it is not the best approach when got to execute multiple functions. That's where promises comes into play. Promises are an another approach for async JS.