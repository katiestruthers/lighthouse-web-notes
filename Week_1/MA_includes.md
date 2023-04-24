# Mentor Assistance: .includes

Mentor recommended using the [.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) JS method to tighten up my function, whereas before I had two loops and a seperate boolean variable I was tracking.

```javascript
const without = function(source, itemsToRemove) {
  const returnArr = [];

  for (let i = 0; i < source.length; i++) {
    if (!itemsToRemove.includes(source[i])) {
      returnArr.push(source[i]);
    }
  }

  return returnArr;
};