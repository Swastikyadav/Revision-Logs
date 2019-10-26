# Network Request: XMLHttpRequest, Fetch

To connect with other networks we use XMLHttpRequest. The build in fetch also uses the same thing under the hood. We will also be creating our own fetch function.

## XMLHttpRequest

```js
var xhr = new XMLHttpRequest;
xhr.open("GET", "https://api.github.com/users/swastikyadav");
xhr.onload = () => console.log(xhr.response);
xhr.onerror = () => console.log("Err Occurred!");
xhr.send();

// Output: { ... }
```

We just got some data from an another newtwork (github). Now let's create our own fetch function.

## XHR + Promise = Fetch

XMLHttpRequest used with promise makes fetch. And as with promise we can use `.then` to access fetched data. 

```js
function fetchData(url) {
    var promise = new Promise((res, rej) => {
        let xhr = new XMLHttpRequest;
        xhr.open("GET", url);
        xhr.onload = () => res(xhr.response);
        xhr.onerror = () => rej(new Error());
        xhr.send();
    });
    return promise;
}

fetchData('https://api.github.com/users/swastikyadav')
    .then(res => JSON.parse(res))
    .then(data => console.log(data.followers));

// Output: 11 followers
```

That was our own fetch function. Now let's work with build in fetch function.

## Fetch (Build in)

```js
fetchData('https://api.github.com/users/swastikyadav')
    .then(res => res.json())
    .then(data => console.log(data.followers));

// Output: 11 followers
```

Build in Fetch function may not have support in all browsers. But our custom fetch function, that will work everywhere. Hence many developers prefer to use their custom fetch function.

## THE END