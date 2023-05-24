# Lecture 7: Promises

### What problems can Async solve?
* Reaching out to an API
* A timeout or interval
* Saving or reading from a database
* Reading or writing a file

### What has been our solution so far?
* CALLBACKS! &rarr; higher-order function that takes in a function as an argument

### Another Solution: Promises!
* An object of class 'Promise'
* Promises exist in one of three possible states:
  1. Pending
  2. Fulfilled
  3. Rejected

```js
// A Promise object takes in a function with two parameters
const myPromise = new Promise((resolve, reject) => {
  const randNum = Math.random();

  if (randNum > 0.5) {
    resolve('Success!');
  } else {
    reject('Failure!');
  }
});

myPromise
  .then((message) => { // Will only run once myPromise has finished executing
    console.log(message);
  });
  .catch((error) => { // Like a try-catch, will catch any errors / rejects in myPromise or .then
    console.log(error);
  });
```

### MadLibs Program using Callbacks
```js
const readline = require('readline'); // if using Callbacks, only need this simpler require
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.question('How do you feel about NodeJS? ', (nodeJS) => {
  rl.question('What\'s your name? ', (name) => {
    rl.question('What\'s your favourite activity? ', (activity) => {
      console.log(`My name is ${name}, I feel ${nodeJS} about NodeJS, and my favourite acitivity is ${activity}.`)
      rl.close();
    });
  });
});
```
**Big Takeaway:** NESTING! Aka, the Callback Waterfall, or less eloquently, the Callback Hell

### MadLibs Program using Promises
```js
const readline = require('readline-promise').default; // if using Promises, require this instead!
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

let nodeJS, name, activity;

rl.questionAsync('How do you feel about NodeJS? ');
  .then((answer) => {
    nodeJS = answer;
    return r1.questionAsync('What\'s your name? ');
  })
  .then((answer) => {
    name = answer;
    return r1.questionAsync('What\'s your favourite activity? ');
  })
  .then((answer) => {
    activity = answer;
    console.log(`My name is ${name}, I feel ${nodeJS} about NodeJS, and my favourite acitivity is ${activity}.`)
    rl.close();
  })
  .catch(error) => {
    console.log('An error occurred.');
  }
```

**Big Takeaway:** While both solutions work, using chained callbacks can get hectic quickly the further into nesting you go. This is why many people prefer using promises.

### Using Timeouts with Promises
```js
const timeoutPromise = (message, shouldWeReject=false) => {
  return new Promise ((resolve, reject) => {
    if (shouldWeReject) {
      reject('Promise failed.');
    }

    const randomMs = Math.floor(Math.random() * 3000);
    setTimeout(() => {
      resolve(message);
    }, randomMs);
  });
};
```

### Promise.all()
```js
const promise1 = timeoutPromise('Timeout Promise 1');
const promise2 = timeoutPromise('Timeout Promise 2');
const promise3 = timeoutPromise('Timeout Promise 3');

Promise.all(promise1, promise2, promise3)
  .then((arrayOfPromiseResponses) => { // Will only execute once all promises are finished
    console.log(
      `All promises are complete:\r\n
      \t${arrayOfPromiseResponses[0]}\n
      \t${arrayOfPromiseResponses[1]}\n
      \t${arrayOfPromiseResponses[2]}\n`
      )
  });
```
* Even though timeoutPromise will give back a random number for each promise, they will still execute in the order they were put in for Promise.all()

### Promise.race()
```js
Promise.all(promise1, promise2, promise3)
  .then((firstResponse) => { // Will only execute once just ONE promise is finished
    console.log(
      `First response:\r\n
      \t${firstResponse}`
      )
  });
```

* Here we will see the randomness, as now we will only see the fastest promise

### .readFile()
```js
const fs = require('fs');
const fsPromises = fs.promises;

fsPromises.readFile('./notes.txt')
  .then((result) => {
    console.log(String(result));
  });
  .catch((error) => {
    console.log('File not found or unreadable.');
  });
```

### .writeFile()
```js
const fs = require('fs');
const fsPromises = fs.promises;

const writeOptions = {
  encoding: 'utf-8',
  flag: 'w'
};

fsPromises.writeFile(
  './notes.txt', // Target file
  'JavaScript was here. :)', // Content to write to file
  writeOptions // Options for write
).then((result) => {
    console.log('File is written successfully!');
}).catch((error) => {
    console.log('File not found or unwritable.');
});
```