# Lecture 5: Async Control Flow

### Async vs. Sync
* Async: code that runs in the background, that doesn't stop the rest of the code from running
  * Good rule of thumb: if its async, it will run at the end!

* Certain functions JS will automatically treat as async, such as setTimeout(), setInterval(), or events

### High Order Functions
Functions that either:
* Returns a function, and / or
* Accepts a function as a parameter

Examples: Array.forEach(), Array.map(), setTimeout(), setInterval

Multiple Ways of Passing in Functions:
```js
// Anonymous Function
setInterval(() => {

});

// Named Function
const callback = () => {

};

setInterval(callback);
```

### setTimeout
* Two parameters:
  1. Callback: what happens (i.e. the function that will execute)
  2. Delay: when it happens (in milliseconds)

```js
setTimeout(callback, delay);
```

```js
setTimeout(() => {
  console.log("this shows up after 3 seconds");
}, 3000);

setTimeout(() => {
  console.log("this shows up first, even though I appear later in the code")
}, 1000);
```

### setInterval (& clearInterval)
* Two parameters:
  1. Callback: what happens (i.e. the function that will execute)
  2. Interval: how often it happens (in milliseconds)

* clearInterval stops the interval from continuing in an infinite loop

```js
setInterval(callback, interval);
```

```js
let seconds = 0;
const timer = setInterval(() => {
  console.log(`${seconds} seconds have gone by`);
  seconds++;

  if (seconds >= 10) {
    clearInterval(timer);
  }
}, 1000);
```

### readFileSync (Sync) vs. readFile (Async)
```js
const fs = require('fs');
const file = fs.readFileSync('./hello.txt', 'utf-8');
console.log("file content using Sync:", file);

fs.readFile('./hello.txt', {encoding: 'utf-8'}, () => {
  console.log("file content using Async:", file);
});
```

### writeFileSync (Sync) vs. writeFile (Async)
```js
const fs = require('fs');
const file = fs.writeFileSync('./hello.txt', 'Sakhia is an awesome teacher');

fs.writeFile('./hello.txt', 'Sakhia is a SUPER awesome teacher', {flag: 'a'} () => {
  console.log('The file has been saved');
});
```

### Events in JS
**TIP:** Want a quick HTML file? &rarr; ! + Tab in VSCode

```js
addEventListener(event, callback);
```

```js
const box = document.getElementById('box');
box.addEventListener('click', () => {
  console.log('the box has been clicked');
});
```

Examples of Events: 'click', 'mouseover', 'mouseout', 'change'