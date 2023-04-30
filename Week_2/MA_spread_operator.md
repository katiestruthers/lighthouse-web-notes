# Mentor Assistance: Spread Operator / Rest Parameter

Since the problem had to account for a variable number of potential arguments, using the ...args made for an easy way to account for that.

```js
const wrapLog = function(callback) {
  return function(...args) {
    return callback.apply(null, args);
  };
};
```

The rest parameter can account for the rest of the arguments that were passed into the function. In our case, we used the rest parameter as a catch-all for _all_ of the arguments.

The spread operator, when not being used as a rest parameter, can be used to "open up" an array or object. 