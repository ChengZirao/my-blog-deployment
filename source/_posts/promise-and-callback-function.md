---
title: Promise And Callback Function
date: 2024-02-18 17:39:11
tags: Node.js
---

Promises solve a similar problem to callbacks, but they do so in a much more elegant way.

#### 1) Here is a snippet of a callback function which is similar to Promise:

```javascript
const variable1 = false;
const variable2 = false;

/**
 * A callback function that imitates how Promise work
 * @param {*} callback : Similar to resolve() in Promise
 * @param {*} errorCallback : Similar to reject() in Promise
 */
function callbackFunc(callback, errorCallback) {
  if (variable1) {
    errorCallback({
      name: 'First variable',
      message: 'Reject :(',
    });
  } else if (variable2) {
    errorCallback({
      name: 'Second variable',
      message: 'Reject 💥',
    });
  } else {
    callback('Resolve!');
  }
}

callbackFunc((message) => {
    console.log('Success: ' + message);
  }, (err) => {
    console.log('Error name: ' + err.name);
    console.log('Error message: ' + err.message);
  },
);
```

In this snippet above, the callbackFunc() receives two callback function parameters. If both 'variable1' and 'variable2' are false, the callbackFunc() will be RESOLVED, the result will be:

`Success: Resolve!`

If 'variable1' is true,  the callbackFunc() will be REJECTED, the result will be:

`Error name: First variable`
`Error message: Reject :(`

If 'variable2' is true,  the callbackFunc() will be REJECTED, the result will be:

`Error name: Second variable`
`Error message: Error message: Reject 💥`



#### 2) Next, let's take a look at another snippet of Promise and see what's the difference:

```javascript
function promiseFunc() {
  return new Promise((resolve, reject) => {
    if (variable1) {
      reject({
        name: 'First variable',
        message: 'Reject :(',
      });
    } else if (variable2) {
      reject({
        name: 'Second variable',
        message: 'Reject 💥',
      });
    } else {
      resolve('Resolve!');
    }
  });
}

promiseFunc()
  .then((message) => {
    console.log('Success: ' + message);
  })
  .catch((err) => {
    console.log('Error name: ' + err.name);
    console.log('Error message: ' + err.message);
  });
```

As you can see, the only difference is we changed the parameter name. From 'callback' and 'errorCallback' to 'resolve' and 'reject'. And when we call promiseFunc(), we use then() and catch() instead of passing two callback fuctions as parameter. 

Just making these tweaks, we are now returning a new Promise instead of calling the callbacks.
