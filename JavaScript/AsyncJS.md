# Asynchronous JavaScript, Callbacks, Promises, The Event Loop

**Synchronous** - One after another
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