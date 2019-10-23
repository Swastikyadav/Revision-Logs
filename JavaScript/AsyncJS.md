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