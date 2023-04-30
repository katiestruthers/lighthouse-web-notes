# Mentor Assistance: Accessing Nested Objects

### The Before

```js
const findKey = function(object, callback) {
  for (const item in object) {
    if (callback(item)) {
      return item;
    }
  }
};
```

### The After

```js
const findKey = function(object, callback) {
  for (const item in object) {
    if (callback(object[item])) {
      return item;
    }
  }
};
```

It was helpful to console.log both item and object[item] so that I could properly see the difference and understand it was object[item] I was trying to send into the callback function.