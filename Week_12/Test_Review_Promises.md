# Programming Test: Promises Cheatsheet

### Create a New Promise
```js
const myPromise = new Promise((resolve, reject) => {
  if (success) {
    resolve('It worked!');
  } else {
    reject(new Error('It failed.'));
  }
});
```

### Use a Promise
```js
myPromise
  .then((result) => { // Will run if promise resolves
    console.log(result);
  })
  .catch((error) => { // Will run if promise rejects
    console.log(error);
  });
  .finally(() => { // Will run either way! Use to avoid code reduplication in the .then & .catch
    console.log('Promise completed.');
  })
```

### Chaining a Promise
* Each .then() waits for the previous Promise to be fulfilled before executing, and then in turn returns a new Promise upons its completion.
* In order to move information down the chain, it is important to always return its results. This way you can catch the results in the next .then()'s parameter.
```js
myPromise
  .then((result0) => {
    const result1 = firstFunction();
    return result1;
  })
  .then((result1) => {
    const result2 = secondFunction();
    return result2;
  })
  .then((result2) => {
    const result3 = thirdFunction();
    return result3;
  });
```