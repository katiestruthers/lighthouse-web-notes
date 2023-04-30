# Lecture 3: Callbacks

### Basic Function Declaration

```js
function sayHello() {

}
```

**TIP:** Want to see the inner-workings of a function? Run:

```js
console.log(String(functionName));
```

### Function Expression
```js
const sayHello2() = function() {

};
```

Why this is considered a "cleaner" approach:
  * HOISTING! The function declaration allows it whereas a function expression does not.
  * While hoisting can be a powerful feature, it also makes the code harder to organize.
  * Since function expressions are not hoisted, it will throw an error if code not organized top-down.

 ### Store Functions in an Array
 ```js
const functions = [];
functions.push(sayHello);
 ```

### Higher-Order Functions
A higher-order is any function that follows at least one of two rules:
  1. Accepts a function as an argument, and / or
  2. Returns a function as a result.

 ### Callback
A callback is any function passed into another as an argument.

 ```js
const sayHello = function(name) {
  console.log(`Hello, ${name}!`);
};

const runOurFunction = function(callbackFunction) {
  callBackFunction('runOurFunction');
};

runOurFunction(sayHello);
 ```

 ### Anonymous Functions
An anonymous function is a function that has no name.
* If you only need to use a function once, save the space and time and don't name it!

```js
const runOurFunction = function(callbackFunction) {
  callBackFunction('runOurFunction');
};

runOurFunction(function(text) {
  console.log(`I\'m an anonymous function! Text said: ${text}`);
});
```

### Arrow Functions
```js
const sayHello = (name) => {
  return `Hello, ${name}!`;
};

const sayHelloSlimmer = name => `Hello, ${name}!`;
```

### Our Own Higher-Order Functions
```js
const loopThroughArray = (array, callback) => {
  for (const item of array) {
    callback(item);
  }
};

loopThroughArray(['Cookies', 'Chips', 'Candy'], snack => {
  console.log('A snack I enjoy is:', snack);
});
```

```js
  const map = (array, callback) => {
    const newMappedArr = [];

    for (const item of array) {
      newMappedArr.push(callback(item));
    }

    return newMappedArr;
  };

  const result = map(cities, city => city.toLowerCase());
  console.log(result);
```
