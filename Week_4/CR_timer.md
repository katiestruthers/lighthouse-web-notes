# Code Review: Timer

### The Before
```js
const sorted = process.argv.slice(2).sort((a, b) => a - b);

for (let i = 0; i < sorted.length; i++) {
  if (isNaN(sorted[i]) || sorted[i] < 0) {
    continue;
  }

  setTimeout(() => {
    process.stdout.write('\x07');
  }, sorted[i] * 1000);
}
```

### The Feedback
* Wrap code into a function to improve modularity
* No need to sort! Async will do that automatically

### The After
```js
const input = process.argv.slice(2);

const timer = function(array) {
  for (let i = 0; i < array.length; i++) {
    if (isNaN(array[i]) || array[i] < 0) {
      continue;
    }
  
    setTimeout(() => {
      process.stdout.write('\x07');
    }, array[i] * 1000);
  }
};

timer(input);
```