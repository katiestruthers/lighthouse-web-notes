# Code Review: Improve Lotide w/ JSDoc

### JSDoc

Comments can help improve readability of your code, and using [JSDoc](https://jsdoc.app/about-getting-started.html) comments is one effective method of doing so.

### Example 1: Simple Description

Ensure that you are starting the comment with '/**'.

```js
/** Flatten a nested array. */
const flatten = function(arr) {
  let newArr = [];
  let newArrPos = 0;

  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      for (let j = 0; j < arr[i].length; j++) {
        newArr[newArrPos] = arr[i][j];
        newArrPos++;
      }
    } else {
      newArr[newArrPos] = arr[i];
      newArrPos++;
    }
  }

  return newArr;
};
```

### Example 2: Adding Tags

JSDoc has a list of [tags](https://jsdoc.app/index.html#block-tags) that can be added into the comment for more information.

```js
/** 
 * Flatten a nested array. 
 * @param {array} arr - The given array that may contain nesting.
 * @return {array} newArr - The return array that does not contain nesting.
*/
const flatten = function(arr) {
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      for (let j = 0; j < arr[i].length; j++) {
        newArr[newArrPos] = arr[i][j];
        newArrPos++;
      }
    } else {
      newArr[newArrPos] = arr[i];
      newArrPos++;
    }
  }

  return newArr;
};
```



